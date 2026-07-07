# Day 5. App 개발 실습 2 (Todo + Heropy API) + Vercel 배포 + 최종 평가 대비

## 학습 목표

Day 5가 끝나면 다음을 할 수 있습니다.

- [ ] **Heropy API**의 TODO 엔드포인트(GET/POST/PUT/DELETE)를 호출하는 Todo 앱을 만들 수 있다.
- [ ] 모든 API 요청에 **공통 headers** (`apikey`, `username`)를 붙일 수 있다.
- [ ] **Vercel**에 Vite 프로젝트를 배포하고, 배포된 URL을 확인할 수 있다.
- [ ] 5일 과정을 **스스로 복습**할 수 있고, 최종 평가에 대비할 수 있다.

---

## 1. Heropy TODO API 사용법

강의에서 사용하는 TODO API는 Heropy(박영웅 강사)가 제공하는 연습용 서버입니다.

| 항목 | 값 |
| --- | --- |
| **Base URL** | `https://asia-northeast3-heropy-api.cloudfunctions.net/api/todos` |
| **공통 헤더** | `content-type: application/json` |
|              | `apikey: KDT8_bcAWVpD8` |
|              | `username: KDT8_<YOUR_NAME>` (각자 본인이름으로 변경) |

> ⚠️ `username`은 다른 사람과 겹치면 DB가 섞입니다. **본인 이름이나 닉네임**으로 바꿔서 쓰세요.  
> 형식 예: `KDT8_youngwoong`, `KDT8_minsu`

### 1.1 엔드포인트 정리

| 메서드 | URL | 용도 |
| --- | --- | --- |
| `GET` | `/todos` | 전체 할 일 목록 조회 |
| `POST` | `/todos` | 새 항목 추가 |
| `PUT` | `/todos/:todoId` | 특정 항목 수정 |
| `DELETE` | `/todos/:todoId` | 특정 항목 삭제 |

### 1.2 데이터 타입

```ts
// 응답의 한 항목
interface Todo {
  id: string            // 서버가 만든 고유 ID
  order: number         // 표시 순서
  title: string         // 할 일 제목
  done: boolean         // 완료 여부
  createdAt: string     // 생성 시각 (ISO 문자열)
  updatedAt: string     // 수정 시각 (ISO 문자열)
}

// POST 요청 바디
interface CreateTodoBody {
  title: string         // 필수!
  order?: number        // 선택
}

// PUT 요청 바디
interface UpdateTodoBody {
  title: string         // 필수!
  done: boolean         // 필수!
  order?: number
}
```

### 1.3 curl로 먼저 확인해 보기

터미널(또는 PowerShell)에서 직접 호출해서 응답을 확인해 두면, 코드를 작성할 때 훨씬 수월합니다.

```bash
# 목록 조회
curl https://asia-northeast3-heropy-api.cloudfunctions.net/api/todos \
  -X 'GET' \
  -H 'content-type: application/json' \
  -H 'apikey: KDT8_bcAWVpD8' \
  -H 'username: KDT8_youngwoong'

# 추가
curl https://asia-northeast3-heropy-api.cloudfunctions.net/api/todos \
  -X 'POST' \
  -H 'content-type: application/json' \
  -H 'apikey: KDT8_bcAWVpD8' \
  -H 'username: KDT8_youngwoong' \
  -d '{"title": "KDT 과정 설계 미팅", "order": 0}'

# 수정
curl https://asia-northeast3-heropy-api.cloudfunctions.net/api/todos/<TODO_ID> \
  -X 'PUT' \
  -H 'content-type: application/json' \
  -H 'apikey: KDT8_bcAWVpD8' \
  -H 'username: KDT8_youngwoong' \
  -d '{"title": "KDT 과정 설계 미팅", "done": true}'

# 삭제
curl https://asia-northeast3-heropy-api.cloudfunctions.net/api/todos/<TODO_ID> \
  -X 'DELETE' \
  -H 'content-type: application/json' \
  -H 'apikey: KDT8_bcAWVpD8' \
  -H 'username: KDT8_youngwoong'
```

> Windows PowerShell에서는 `curl` 대신 `Invoke-RestMethod` 또는 `irm`을 써도 됩니다.

---

## 2. axios 인스턴스에 공통 헤더 심기

매번 `apikey`를 적기 귀찮으니, axios 인스턴스에 **기본값으로 박아 둡니다**.

```ts
// src/api/instance.ts
import axios from 'axios'

const username = 'KDT8_youngwoong'  // ← 본인 이름으로 변경

export const api = axios.create({
  baseURL: 'https://asia-northeast3-heropy-api.cloudfunctions.net/api',
  headers: {
    'content-type': 'application/json',
    'apikey': 'KDT8_bcAWVpD8',
    'username': username,
  },
})
```

이제 어디서 `api.get('/todos')`만 부르면 headers가 자동으로 따라갑니다.

---

## 3. Todo API 모듈

```ts
// src/api/todo.ts
import { api } from './instance'

export interface Todo {
  id: string
  order: number
  title: string
  done: boolean
  createdAt: string
  updatedAt: string
}

export interface CreateTodoBody {
  title: string
  order?: number
}

export interface UpdateTodoBody {
  title: string
  done: boolean
  order?: number
}

// 목록
export async function fetchTodos(): Promise<Todo[]> {
  const { data } = await api.get<Todo[]>('/todos')
  return data
}

// 추가
export async function createTodo(body: CreateTodoBody): Promise<Todo> {
  const { data } = await api.post<Todo>('/todos', body)
  return data
}

// 수정
export async function updateTodo(
  id: string,
  body: UpdateTodoBody,
): Promise<Todo> {
  const { data } = await api.put<Todo>(`/todos/${id}`, body)
  return data
}

// 삭제
export async function deleteTodo(id: string): Promise<true> {
  const { data } = await api.delete<true>(`/todos/${id}`)
  return data
}
```

---

## 4. Zustand 스토어 — 비동기 액션

Day 3에서는 동기 액션(`add`, `toggle`)만 있었지만, Day 5에서는 **API 호출이 들어간 비동기 액션**을 다룹니다.

```ts
// src/stores/useTodoStore.ts
import { create } from 'zustand'
import {
  fetchTodos,
  createTodo,
  updateTodo,
  deleteTodo,
  type Todo,
} from '../api/todo'

type Status = 'idle' | 'loading' | 'success' | 'error'

interface TodoState {
  todos: Todo[]
  input: string
  status: Status
  error: string
  setInput: (v: string) => void
  load: () => Promise<void>
  add: () => Promise<void>
  toggle: (id: string) => Promise<void>
  edit: (id: string, title: string) => Promise<void>
  remove: (id: string) => Promise<void>
}

export const useTodoStore = create<TodoState>((set, get) => ({
  todos: [],
  input: '',
  status: 'idle',
  error: '',

  setInput: (v) => set({ input: v }),

  // 목록 불러오기
  load: async () => {
    set({ status: 'loading', error: '' })
    try {
      const todos = await fetchTodos()
      set({ status: 'success', todos })
    } catch (e) {
      set({ status: 'error', error: msg(e) })
    }
  },

  // 추가
  add: async () => {
    const title = get().input.trim()
    if (!title) return
    try {
      const todo = await createTodo({ title })
      set((s) => ({ todos: [...s.todos, todo], input: '' }))
    } catch (e) {
      set({ error: msg(e) })
    }
  },

  // 완료 토글
  toggle: async (id) => {
    const target = get().todos.find((t) => t.id === id)
    if (!target) return
    try {
      const updated = await updateTodo(id, {
        title: target.title,
        done: !target.done,
      })
      set((s) => ({
        todos: s.todos.map((t) => (t.id === id ? updated : t)),
      }))
    } catch (e) {
      set({ error: msg(e) })
    }
  },

  // 제목 수정
  edit: async (id, title) => {
    const target = get().todos.find((t) => t.id === id)
    if (!target) return
    try {
      const updated = await updateTodo(id, {
        title,
        done: target.done,
      })
      set((s) => ({
        todos: s.todos.map((t) => (t.id === id ? updated : t)),
      }))
    } catch (e) {
      set({ error: msg(e) })
    }
  },

  // 삭제
  remove: async (id) => {
    try {
      await deleteTodo(id)
      set((s) => ({ todos: s.todos.filter((t) => t.id !== id) }))
    } catch (e) {
      set({ error: msg(e) })
    }
  },
}))

function msg(e: unknown): string {
  return e instanceof Error ? e.message : '알 수 없는 오류'
}
```

> 💡 `get()`을 쓰면 **현재 store 값**을 함수 안에서 동기적으로 읽을 수 있습니다. `set`은 state를 갱신, `get`은 읽기 — 이 둘을 구분해서 쓰세요.

---

## 5. Todo 페이지 — 마운트 시 자동 로드

```tsx
// src/pages/Todo.tsx
import { useEffect, useState } from 'react'
import { useTodoStore } from '../stores/useTodoStore'

export default function Todo() {
  const {
    todos, input, status, error,
    setInput, load, add, toggle, remove,
  } = useTodoStore()

  // 화면에 처음 들어오면 자동으로 서버에서 목록을 불러옴
  useEffect(() => {
    load()
  }, [load])

  return (
    <div className="max-w-md mx-auto p-4">
      <h1 className="text-2xl font-bold mb-4">📝 Todo (Server)</h1>

      {error && (
        <p className="mb-2 p-2 bg-red-100 text-red-700 rounded">{error}</p>
      )}

      <div className="flex gap-2 mb-4">
        <input
          className="border px-2 py-1 rounded flex-1"
          value={input}
          onChange={(e) => setInput(e.target.value)}
          onKeyDown={(e) => e.key === 'Enter' && add()}
          placeholder="할 일을 입력하세요"
        />
        <button
          onClick={add}
          className="bg-blue-500 text-white px-3 py-1 rounded"
        >
          추가
        </button>
      </div>

      {status === 'loading' && todos.length === 0 && (
        <p className="text-gray-500">불러오는 중...</p>
      )}

      <ul className="border rounded">
        {todos.length === 0 && status === 'success' && (
          <li className="p-4 text-center text-gray-400">할 일이 없어요 🎉</li>
        )}
        {todos.map((t) => (
          <TodoItem key={t.id} todo={t} onToggle={toggle} onRemove={remove} />
        ))}
      </ul>
    </div>
  )
}

interface TodoItemProps {
  todo: { id: string; title: string; done: boolean }
  onToggle: (id: string) => void
  onRemove: (id: string) => void
}

function TodoItem({ todo, onToggle, onRemove }: TodoItemProps) {
  const [editing, setEditing] = useState(false)
  const [draft, setDraft] = useState(todo.title)

  if (editing) {
    return (
      <li className="flex items-center gap-2 p-2 border-b">
        <input
          className="border px-2 py-1 rounded flex-1"
          value={draft}
          onChange={(e) => setDraft(e.target.value)}
          onKeyDown={(e) => {
            if (e.key === 'Enter') {
              // enter로 저장
              setEditing(false)
              if (draft.trim()) {
                // 부모의 edit이 있으면 호출 (생략)
              }
            }
          }}
        />
        <button onClick={() => setEditing(false)} className="text-sm">
          취소
        </button>
      </li>
    )
  }

  return (
    <li className="flex items-center gap-2 p-2 border-b">
      <input
        type="checkbox"
        checked={todo.done}
        onChange={() => onToggle(todo.id)}
      />
      <span
        onDoubleClick={() => setEditing(true)}
        className={
          todo.done ? 'line-through text-gray-400 flex-1 cursor-pointer' : 'flex-1 cursor-pointer'
        }
      >
        {todo.title}
      </span>
      <button onClick={() => onRemove(todo.id)} className="text-red-500 text-sm">
        삭제
      </button>
    </li>
  )
}
```

---

## 6. Vercel로 배포하기

Vercel은 **프론트엔드 전용 호스팅 서비스**입니다. GitHub 저장소를 연결하면 `git push`만으로 자동 배포됩니다.

### 6.1 배포 3단계

```txt
1) GitHub에 프로젝트 저장소 만들기 + 코드 push
2) vercel.com 가입 (GitHub 계정으로 로그인)
3) "New Project" → 저장소 선택 → "Deploy" 클릭 → 끝!
```

### 6.2 배포 전 체크리스트

- [ ] `npm run build`가 로컬에서 **에러 없이** 끝나는가
- [ ] `package.json`의 `"scripts"`에 `build`가 있는가 (Vite 템플릿은 기본 포함)
- [ ] API 키 등 **민감 정보**가 코드에 하드코딩되어 있지 않은가
- [ ] `.env` 파일은 `.gitignore`에 등록되어 있는가

### 6.3 빌드 미리 테스트

```bash
npm run build
```

이 명령은 `dist/` 폴더에 배포 가능한 정적 파일을 만들어 줍니다.  
빌드가 성공한다는 것은 "내 코드가 프로덕션 환경에서도 돌아간다"는 뜻입니다.

> 만약 빌드 에러가 나면 보통 (1) TypeScript 타입 오류, (2) 사용하지 않는 import, (3) 환경변수 누락 — 셋 중 하나입니다.

### 6.4 Vercel 환경변수 (선택)

Heropy API 키처럼 **노출되면 안 되는 값**은 Vercel 대시보드의 **Environment Variables**에 등록하고, 코드에서는 `import.meta.env.VITE_xxx`로 읽습니다.

```ts
// src/api/instance.ts
const username = import.meta.env.VITE_USERNAME ?? 'KDT8_guest'
```

Vercel 대시보드:

```txt
Project → Settings → Environment Variables
  VITE_USERNAME = KDT8_youngwoong
```

### 6.5 배포 후 확인

- 배포가 끝나면 `https://<your-app>.vercel.app` 같은 주소가 생깁니다.
- 새로 코드를 push하면 **자동으로 재배포**됩니다 (Preview 배포 → main에 merge하면 Production).

---

## 7. 최종 평가 대비 — 객관식 20문항 모의고사

강의 마지막 1시간은 객관식 20문항입니다. 아래는 5일 과정을 모두 아우르는 **모의 문제 20선**입니다.  
먼저 풀어 보고, 답을 확인하세요.

### 📝 문제

1. TypeScript에서 "문자열" 타입을 나타내는 키워드는?
   ① `String`  ② `str`  ③ `string`  ④ `text`

2. 다음 중 React 컴포넌트 이름 규칙으로 올바른 것은?
   ① `function button() {}`  ② `function Button() {}`
   ③ `BUTTON()`  ④ `function btn() {}`

3. `useState`의 반환값은 어떤 모양인가?
   ① `number`  ② `[value, setter]`  ③ `{ value, set }`  ④ `() => void`

4. Context API의 역할로 가장 적절한 것은?
   ① 전역 변수 선언  ② 컴포넌트 트리 전체에 데이터 공유
   ③ CSS 스타일 격리  ④ 라우팅 처리

5. Zustand에서 **특정 값만 구독**하는 패턴은?
   ① `useStore()`  ② `useStore(state => state.count)`
   ③ `useStore.getState()`  ④ `useStore.subscribe()`

6. React Router에서 **새로고침 없이 페이지 이동**하는 컴포넌트는?
   ① `<a href>`  ② `<Navigate>`  ③ `<Link to>`  ④ `<button>`

7. URL의 쿼리스트링을 읽는 Hook은?
   ① `useParams`  ② `useSearchParams`  ③ `useLocation`  ④ `useQuery`

8. Axios가 fetch와 다른 점으로 옳은 것은?
   ① 수동으로 `.json()`을 호출해야 한다
   ② 4xx/5xx 에러를 자동으로 throw 한다
   ③ Promise를 반환하지 않는다
   ④ GET만 사용할 수 있다

9. `useEffect(() => {...}, [count])`에서 effect가 다시 실행되는 시점은?
   ① 매 렌더링마다  ② 컴포넌트가 사라질 때
   ③ `count`가 바뀔 때  ④ 아무 때나 실행 안 됨

10. 다음 중 **TypeScript의 장점**이 아닌 것은?
    ① 에디터 자동완성  ② 컴파일 타임 오류 잡기
    ③ 런타임 속도 향상  ④ 함수 시그니처 명시

11. JSX에서 `class` 대신 사용하는 속성은?
    ① `className`  ② `class`  ③ `cls`  ④ `kclass`

12. props에 대해 올바른 설명은?
    ① 자식이 자유롭게 변경할 수 있다
    ② 부모가 자식에게 데이터를 전달하는 읽기 전용 값
    ③ 전역 변수다
    ④ 컴포넌트 외부에서도 직접 수정 가능

13. Tailwind CSS에서 `bg-blue-500`이 의미하는 것은?
    ① 파란색 텍스트  ② 파란색 배경
    ③ 파란색 테두리  ④ 파란색 그림자

14. CSS Module에서 클래스명을 가져오는 방법은?
    ① `import './styles.css'`  ② `import styles from './X.module.css'` 후 `styles.button`
    ③ `<button className="button">`  ④ `class="button"`

15. `<Route path="/todo/:id" element={<TodoDetail />}>`에서 id를 읽는 Hook은?
    ① `useId`  ② `useParams`  ③ `useLocation`  ④ `useRouteId`

16. 다음 중 **fetch의 응답 객체**에서 JSON 본문을 얻는 메서드는?
    ① `response.text()`  ② `response.data()`
    ③ `response.body()`  ④ `response.json()`

17. Promise를 `await`로 기다리려면 함수가 무엇이어야 하는가?
    ① 화살표 함수  ② `async` 함수
    ③ 제너레이터 함수  ④ 콜백 함수

18. Vercel로 Vite 앱을 배포할 때 **필수 사전 작업**은?
    ① Vercel CLI 설치  ② 도메인 구매
    ③ `npm run build`가 로컬에서 성공하는지 확인  ④ SSL 인증서 발급

19. Zustand에서 **현재 상태를 함수 안에서 동기적으로** 읽는 함수는?
    ① `set`  ② `get`  ③ `use`  ④ `read`

20. 다음 중 **Heropy TODO API의 GET `/todos`** 응답 타입으로 옳은 것은?
    ① `Todo`  ② `Todo[]`  ③ `string`  ④ `boolean`

### ✅ 정답

| # | 정답 | 해설 |
| --- | --- | --- |
| 1 | ③ | TypeScript 원시 타입은 소문자 `string`, `number`, `boolean` |
| 2 | ② | 컴포넌트 함수는 반드시 **대문자**로 시작해야 JSX로 인식됨 |
| 3 | ② | `const [value, setValue] = useState(0)` 형태의 배열 |
| 4 | ② | props drilling 없이 트리 전체에서 데이터 공유 |
| 5 | ② | selector: `useStore(state => state.count)` |
| 6 | ③ | `<Link to>`는 SPA 방식, `<a href>`는 페이지 새로고침 |
| 7 | ② | `const [params, setParams] = useSearchParams()` |
| 8 | ② | fetch는 수동으로 `.ok` 확인 필요, axios는 자동 throw |
| 9 | ③ | 의존성 배열의 값이 바뀔 때마다 effect 재실행 |
| 10 | ③ | TypeScript는 **런타임에 제거**됨 (성도 향상 X) |
| 11 | ① | JSX에서는 HTML과 다르게 `className` 사용 |
| 12 | ② | props는 부모→자식, 읽기 전용 |
| 13 | ② | `bg-` 접두사는 배경, `text-`는 글자색 |
| 14 | ② | `import styles from './X.module.css'`, `styles.button` |
| 15 | ② | `useParams().id` |
| 16 | ④ | `await response.json()` |
| 17 | ② | `await`는 `async` 함수 내부에서만 사용 가능 |
| 18 | ③ | 빌드가 깨지면 Vercel 배포도 실패 |
| 19 | ② | `set`은 쓰기, `get`은 읽기 |
| 20 | ② | `type ResponseValue = Todo[]` (강의 노션 명세) |

---

## 8. 자주 하는 실수 체크리스트

- [ ] Heropy API 호출 시 `username`이 다른 사람과 겹쳐 데이터가 섞임 → 본인 이름으로 변경
- [ ] `useEffect(() => load(), [])`처럼 빈 배열로 마운트 1회만 실행하려는데, `load`가 매번 새로 만들어져 무한 루프 → `load`를 의존성에 넣기
- [ ] `dist/` 폴더를 git에 올림 → `.gitignore`에 추가
- [ ] API 키를 그대로 커밋 → 환경변수로 분리
- [ ] Vercel 배포 후 캐시 때문에 변경이 안 보임 → 하드 새로고침(Ctrl + Shift + R)

---

## 9. 5일 전체 요약 — 큰 그림

```txt
[Day 1] Vite + React + TS 프로젝트 시작
        ↓
[Day 2] 컴포넌트 분리 + Tailwind/CSS Module + Context API
        ↓
[Day 3] Zustand(상태) + React Router(페이지)
        ↓
[Day 4] React Router 심화(useSearchParams) + Axios + 영화 검색 앱
        ↓
[Day 5] Heropy API + Todo 앱 + Vercel 배포 + 최종 평가
```

**가장 자주 등장한 5가지 패턴**

| 패턴 | 의미 |
| --- | --- |
| `interface XProps { ... }` | Props의 모양을 미리 선언 |
| `useState`, `useEffect` | 컴포넌트의 기억과 부수효과 |
| `const [s, setS] = useState(0)` | 상태와 갱신 함수의 분리 |
| `useStore((s) => s.x)` | selector로 필요한 것만 구독 |
| `await api.get('/url')` + try/catch | API 호출의 기본 형태 |

---

## 10. 다음 단계 (강의 이후 학습 로드맵)

5일이 지나면, 다음 주제를 차례로 살펴보면 좋습니다.

1. **React Hooks 심화** — `useMemo`, `useCallback`, `useRef`, 커스텀 훅 (참고: [React Hooks 핵심 정리](https://www.heropy.dev/p/revOrg))
2. **데이터 패칭 라이브러리** — TanStack Query (서버 상태 관리)
3. **테스팅** — Vitest, Playwright
4. **Next.js** — 서버 사이드 렌더링, 라우팅/데이터 패칭의 새로운 패러다임
5. **TypeScript 심화** — 제네릭, 유틸리티 타입, `satisfies`

> 💡 본 노트에 적힌 내용은 모두 "첫 발"입니다. **어떤 라이브러리/도구가 어떤 문제를 풀려고 등장했는가?**를 이해하는 것이 장기적으로 가장 중요합니다.

---

## 부록. 최종 폴더 구조 예시

5일 과정을 모두 따라 만든 프로젝트의 모습입니다.

```txt
my-app/
├── public/
├── src/
│   ├── api/
│   │   ├── instance.ts        ← axios 인스턴스 (공통 headers)
│   │   ├── movie.ts           ← OMDb 영화 검색
│   │   └── todo.ts            ← Heropy TODO CRUD
│   ├── components/
│   │   ├── Header.tsx
│   │   ├── Button.tsx
│   │   └── TodoItem.tsx
│   ├── contexts/
│   │   └── ThemeContext.tsx   ← Day 2 다크모드 Context
│   ├── pages/
│   │   ├── Home.tsx
│   │   ├── About.tsx
│   │   ├── Movies.tsx
│   │   ├── MovieDetail.tsx
│   │   ├── Todo.tsx
│   │   └── NotFound.tsx
│   ├── stores/
│   │   ├── useCounterStore.ts
│   │   ├── useMovieStore.ts
│   │   └── useTodoStore.ts
│   ├── App.tsx                ← 라우터
│   ├── main.tsx               ← 진입점
│   └── index.css              ← tailwindcss import
├── .env                       ← 환경변수 (git에 안 올라감)
├── index.html
├── package.json
├── tsconfig.json
└── vite.config.ts
```

축하합니다! 🎉  
이 폴더 구조가 자연스럽게 만들어진다면, 5일 과정을 잘 소화한 것입니다.

---

## 다음 단계

강의에서 배운 모든 패턴이 모이면 **자신만의 작은 프로젝트**(영화 검색 + Todo 통합, 간단한 블로그 등)를 만들 수 있습니다.  
👉 [최상위 README로 돌아가기](./../README.md)