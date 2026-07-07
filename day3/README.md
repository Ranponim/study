# Day 3. Zustand 상태 관리 + React Router 라우팅 기초

## 학습 목표

Day 3이 끝나면 다음을 할 수 있습니다.

- [ ] **Zustand**를 설치하고, `store`를 만들어 여러 컴포넌트가 공유하는 상태를 관리할 수 있다.
- [ ] Context API와 Zustand의 **차이**를 설명할 수 있다.
- [ ] **React Router**로 여러 페이지(`/`, `/about`, `/todo`)를 가진 SPA를 만들 수 있다.
- [ ] **`<Link>`**와 **`useNavigate`**로 페이지를 이동할 수 있다.

---

## 1. 상태 관리 — 왜 "외부 저장소"가 필요한가?

### 1.1 Context API의 한계

Day 2에서 Context API로 테마를 공유해 봤습니다. 잘 동작하죠.  
하지만 상태가 커지면 문제가 생깁니다.

| 상황 | Context | Zustand |
| --- | --- | --- |
| Provider 안에서만 사용 가능 | ✅ | ✅ (파일 어디서든) |
| 작은 상태 (테마, locale) | 👍 | 👍 |
| 자주 바뀌는 큰 상태 (리스트, 폼 입력) | 🐢 성능 이슈 가능 | 🚀 빠름 |
| 상태 변경 함수 위치 | 컴포넌트 안 | **store**에 한곳에 모음 |
| 디버깅 도구 | 직접 구현 | Zustand devtools 지원 |

> 💡 **한 줄 요약**: Context는 "데이터 공유"에, Zustand는 "데이터 + 변경 함수 + 성능"까지 챙기는 도구입니다.

### 1.2 Zustand 설치

```bash
npm install zustand
```

---

## 2. Zustand 핵심 패턴

### 2.1 가장 기본 — `create`

```tsx
// src/stores/useCounterStore.ts
import { create } from 'zustand'

// 1) 상태(state)의 모양과 변경 함수를 한꺼번에 정의
interface CounterState {
  count: number
  increase: () => void
  decrease: () => void
  reset: () => void
}

// 2) create()로 store 만들기
export const useCounterStore = create<CounterState>((set) => ({
  count: 0,
  increase: () => set((s) => ({ count: s.count + 1 })),
  decrease: () => set((s) => ({ count: s.count - 1 })),
  reset: () => set({ count: 0 }),
}))
```

> `set`은 "상태를 갱신하는 함수"입니다. `set(부분객체)`로 합칠 수도, `set((s) => ...)`로 함수형 갱신도 가능합니다.  
> JS의 `Array.map`처럼 (참고: `../javascript/10-배열과객체-메서드`), `s`는 현재 상태를 가리킵니다.

### 2.2 컴포넌트에서 사용

```tsx
import { useCounterStore } from './stores/useCounterStore'

function Counter() {
  // 전체 store 구독 (count가 바뀔 때마다 재렌더링)
  const { count, increase, decrease, reset } = useCounterStore()

  return (
    <div className="p-4">
      <p>count = {count}</p>
      <button onClick={increase}>+1</button>
      <button onClick={decrease}>-1</button>
      <button onClick={reset}>초기화</button>
    </div>
  )
}
```

어디서든 import만 하면 쓸 수 있습니다. Provider로 감쌀 필요 없음! 🎉

### 2.3 Selector — "필요한 값만 골라 듣기"

`useCounterStore()`를 호출하면 store의 **모든 값**이 바뀔 때마다 컴포넌트가 다시 그려집니다.  
값이 많을 때는 **selector**로 필요한 것만 꺼내면 불필요한 재렌더링을 막을 수 있습니다.

```tsx
function CountDisplay() {
  // count만 구독. increase 등이 바뀌어도 재렌더링 안 함
  const count = useCounterStore((s) => s.count)
  return <p>현재 카운트: {count}</p>
}

function IncreaseButton() {
  // 함수만 구독. count가 바뀌어도 재렌더링 안 함
  const increase = useCounterStore((s) => s.increase)
  return <button onClick={increase}>+1</button>
}
```

> 💡 **성능 팁**: 컴포넌트에서 store를 쓸 때는 항상 selector를 먼저 고려하세요. 전체 객체를 구조분해하면 모든 변경에 재렌더링됩니다.

### 2.4 Todo store — 조금 더 실용적인 예제

```tsx
// src/stores/useTodoStore.ts
import { create } from 'zustand'

export interface Todo {
  id: number
  title: string
  done: boolean
}

interface TodoState {
  todos: Todo[]
  input: string
  setInput: (v: string) => void
  add: () => void
  toggle: (id: number) => void
  remove: (id: number) => void
}

export const useTodoStore = create<TodoState>((set) => ({
  todos: [
    { id: 1, title: 'Zustand 공부', done: false },
    { id: 2, title: 'React Router 맛보기', done: false },
  ],
  input: '',
  setInput: (v) => set({ input: v }),
  add: () =>
    set((s) => {
      if (!s.input.trim()) return s
      const newTodo: Todo = {
        id: Date.now(),
        title: s.input.trim(),
        done: false,
      }
      return { todos: [...s.todos, newTodo], input: '' }
    }),
  toggle: (id) =>
    set((s) => ({
      todos: s.todos.map((t) => (t.id === id ? { ...t, done: !t.done } : t)),
    })),
  remove: (id) =>
    set((s) => ({ todos: s.todos.filter((t) => t.id !== id) })),
}))
```

`App.tsx`에서 사용:

```tsx
import { useTodoStore } from './stores/useTodoStore'

function TodoInput() {
  const input = useTodoStore((s) => s.input)
  const setInput = useTodoStore((s) => s.setInput)
  const add = useTodoStore((s) => s.add)

  return (
    <div className="flex gap-2">
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
  )
}

function TodoList() {
  const todos = useTodoStore((s) => s.todos)
  const toggle = useTodoStore((s) => s.toggle)
  const remove = useTodoStore((s) => s.remove)

  return (
    <ul className="mt-4">
      {todos.map((t) => (
        <li key={t.id} className="flex items-center gap-2 p-2 border-b">
          <input
            type="checkbox"
            checked={t.done}
            onChange={() => toggle(t.id)}
          />
          <span className={t.done ? 'line-through text-gray-400 flex-1' : 'flex-1'}>
            {t.title}
          </span>
          <button
            onClick={() => remove(t.id)}
            className="text-red-500 text-sm"
          >
            삭제
          </button>
        </li>
      ))}
    </ul>
  )
}
```

이렇게 하면 **Input, List가 서로 부모-자식 관계가 아니어도 같은 데이터를 공유**합니다.

---

## 3. React Router — 여러 페이지를 가진 SPA

### 3.1 라우팅이란?

브라우저 주소창의 URL에 따라 다른 화면을 보여주는 것.

- **`/`** → 홈
- **`/about`** → 소개
- **`/todo/3`** → 3번 할 일 상세

리액트 자체에는 라우팅 기능이 없어서, **React Router**라는 외부 라이브러리를 씁니다. (참고: [React Router 핵심 정리](https://www.heropy.dev/p/9tesDt))

### 3.2 설치

```bash
npm install react-router-dom
```

### 3.3 가장 기본 — `BrowserRouter`, `Routes`, `Route`

```tsx
// src/main.tsx
import { BrowserRouter } from 'react-router-dom'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </StrictMode>,
)
```

```tsx
// src/App.tsx
import { Routes, Route } from 'react-router-dom'
import Home from './pages/Home'
import About from './pages/About'
import NotFound from './pages/NotFound'

export default function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
      <Route path="*" element={<NotFound />} />
    </Routes>
  )
}
```

이제 `/`로 가면 `<Home />`이, `/about`으로 가면 `<About />`이 렌더링됩니다.

### 3.4 페이지 이동 — `<Link>`

HTML의 `<a href>`는 페이지를 새로고침하면서 이동합니다.  
React Router의 `<Link>`는 **새로고침 없이 URL만 바꿔**줍니다.

```tsx
import { Link } from 'react-router-dom'

function Header() {
  return (
    <nav className="flex gap-4 p-4 border-b">
      <Link to="/" className="hover:underline">홈</Link>
      <Link to="/about" className="hover:underline">소개</Link>
    </nav>
  )
}
```

> ⚠️ `<a href>`와 `<Link to>`의 차이:  
> - `<a href>` → 페이지 전체를 새로 받아옴 (느림, 상태 초기화)  
> - `<Link to>` → 필요한 부분만 바꿔치기 (빠름, 상태 유지)

### 3.5 코드 안에서 이동 — `useNavigate`

버튼 클릭이나 폼 제출 후에는 `useNavigate`로 이동합니다.

```tsx
import { useNavigate } from 'react-router-dom'

function Home() {
  const navigate = useNavigate()

  return (
    <div className="p-4">
      <h1>홈 페이지</h1>
      <button
        onClick={() => navigate('/about')}
        className="mt-2 px-3 py-1 bg-blue-500 text-white rounded"
      >
        소개 페이지로 가기
      </button>
    </div>
  )
}
```

### 3.6 URL 파라미터 — 동적 라우트

`/todo/3`처럼 URL에 값이 들어가는 라우트입니다.

```tsx
// App.tsx
<Route path="/todo/:id" element={<TodoDetail />} />
```

```tsx
// pages/TodoDetail.tsx
import { useParams } from 'react-router-dom'

export default function TodoDetail() {
  const { id } = useParams()        // id는 string | undefined
  return <p>할 일 ID: {id}</p>
}
```

### 3.7 SPA 구조 그림

```txt
브라우저 URL:  https://myapp.com/todo/3

┌────────────────────────────────────┐
│ index.html (단 1개)                │
│  ┌──────────────────────────────┐  │
│  │  React App                   │  │
│  │  ┌────────────────────────┐  │  │
│  │  │ Header (Link)          │  │  │
│  │  ├────────────────────────┤  │  │
│  │  │ URL: /todo/3           │  │  │
│  │  │ 매칭: <TodoDetail id=3>│  │  │
│  │  └────────────────────────┘  │  │
│  └──────────────────────────────┘  │
└────────────────────────────────────┘
```

서버는 `index.html`만 내려주고, **어떤 컴포넌트를 그릴지는 React가 URL 보고 결정**합니다.  
이것이 SPA(Single Page Application)의 핵심입니다.

---

## 4. 실습 — 3개 페이지 라우터

**`src/pages/Home.tsx`**

```tsx
import { useNavigate } from 'react-router-dom'

export default function Home() {
  const navigate = useNavigate()
  return (
    <div className="p-6">
      <h1 className="text-3xl font-bold">🏠 Home</h1>
      <p className="mt-2 text-gray-600">환영합니다!</p>
      <button
        onClick={() => navigate('/todo')}
        className="mt-4 px-3 py-1 bg-blue-500 text-white rounded"
      >
        Todo 페이지로 →
      </button>
    </div>
  )
}
```

**`src/pages/About.tsx`**

```tsx
export default function About() {
  return (
    <div className="p-6">
      <h1 className="text-3xl font-bold">ℹ️ About</h1>
      <p className="mt-2 text-gray-600">이 앱은 Day 3 실습용입니다.</p>
    </div>
  )
}
```

**`src/pages/Todo.tsx`** — Zustand로 만든 Todo 페이지

```tsx
import { useTodoStore } from '../stores/useTodoStore'

export default function Todo() {
  const todos = useTodoStore((s) => s.todos)
  const input = useTodoStore((s) => s.input)
  const setInput = useTodoStore((s) => s.setInput)
  const add = useTodoStore((s) => s.add)
  const toggle = useTodoStore((s) => s.toggle)
  const remove = useTodoStore((s) => s.remove)

  return (
    <div className="max-w-md mx-auto p-4">
      <h1 className="text-2xl font-bold mb-4">📝 Todo</h1>

      <div className="flex gap-2">
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

      <ul className="mt-4 border rounded">
        {todos.length === 0 && (
          <li className="p-4 text-center text-gray-400">
            할 일이 없어요 🎉
          </li>
        )}
        {todos.map((t) => (
          <li key={t.id} className="flex items-center gap-2 p-2 border-b">
            <input
              type="checkbox"
              checked={t.done}
              onChange={() => toggle(t.id)}
            />
            <span
              className={
                t.done ? 'line-through text-gray-400 flex-1' : 'flex-1'
              }
            >
              {t.title}
            </span>
            <button
              onClick={() => remove(t.id)}
              className="text-red-500 text-sm"
            >
              삭제
            </button>
          </li>
        ))}
      </ul>
    </div>
  )
}
```

**`src/components/Header.tsx`**

```tsx
import { Link } from 'react-router-dom'

export default function Header() {
  return (
    <nav className="flex gap-4 p-4 border-b bg-white">
      <Link to="/" className="hover:underline font-semibold">Home</Link>
      <Link to="/about" className="hover:underline">About</Link>
      <Link to="/todo" className="hover:underline">Todo</Link>
    </nav>
  )
}
```

**`src/App.tsx`**

```tsx
import { Routes, Route } from 'react-router-dom'
import Header from './components/Header'
import Home from './pages/Home'
import About from './pages/About'
import Todo from './pages/Todo'
import NotFound from './pages/NotFound'

export default function App() {
  return (
    <div>
      <Header />
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/todo" element={<Todo />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </div>
  )
}
```

**`src/pages/NotFound.tsx`**

```tsx
export default function NotFound() {
  return (
    <div className="p-6 text-center">
      <h1 className="text-3xl font-bold">404</h1>
      <p className="text-gray-600">페이지를 찾을 수 없습니다.</p>
    </div>
  )
}
```

---

## 5. 자주 하는 실수 체크리스트

- [ ] `BrowserRouter`로 `<App />`을 감싸지 않아서 `Link`, `useNavigate`가 동작 안 함
- [ ] `<a href>`를 써서 새로고침이 일어남 → `<Link to>`로 바꿔야 함
- [ ] store 전체를 구독해서 입력 한 글자마다 리스트가 재렌더링됨 → selector 사용
- [ ] `set`에 새 객체를 통째로 넣음 (`set({ count: 0 })`만 넣어야 다른 필드가 사라지지 않음)
- [ ] `<Route path="/todo">`에서 `useParams()`를 호출해서 `id`를 받으려 함 → 동적 라우트(`/:id`)가 아니면 안 잡힘

---

## 6. 연습 문제

### 문제 1. 카운터 store

`count`, `increase`, `decrease`, `set(value: number)`를 가진 `useCounterStore`를 만들고, 헤더에 현재 카운트와 +/- 버튼을 보여주는 컴포넌트를 작성해 보세요.

### 문제 2. NotFound 페이지

없는 경로(`/xxx`)로 들어가면 "404 - 페이지를 찾을 수 없습니다" 페이지를 보여주고, "홈으로" `<Link>`를 제공해 보세요.

### 문제 3. 동적 라우트

`/todo/:id`로 들어가면 해당 id의 Todo 제목을 보여주는 `TodoDetail` 페이지를 만들어 보세요. (Hint: `useParams()`, store에서 `todos.find`)

---

## 7. 요약

| 개념 | 한 줄 |
| --- | --- |
| **Zustand** | 가볍고 빠른 상태 관리 라이브러리. store = state + actions |
| **selector** | `useStore((s) => s.x)` 형태로 필요한 값만 구독 |
| **`set`** | 상태를 갱신하는 함수. `set({...})` 또는 `set((s) => ...)` |
| **`BrowserRouter`** | 앱 전체를 라우터로 감싸는 컴포넌트 |
| **`Routes`/`Route`** | URL ↔ 컴포넌트 매칭 |
| **`<Link to>`** | 새로고침 없는 페이지 이동 |
| **`useNavigate`** | 코드 안에서 페이지 이동 |
| **`useParams`** | URL 파라미터(`/todo/:id`) 읽기 |

---

## 8. 다음 단계

Day 4에서는 **영화 검색 앱**을 만들면서 React Router의 더 깊은 기능(`useNavigate`로 검색어 전달)과 **fetch + Axios**를 배웁니다.

👉 [Day 4 노트 열기 — React Router 활용 + 비동기 + App 실습 1](./../day4/README.md)