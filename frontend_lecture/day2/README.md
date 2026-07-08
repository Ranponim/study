# Day 2. React 컴포넌트 + CSS Module / Tailwind CSS + Context API

## 학습 목표

Day 2가 끝나면 다음을 할 수 있습니다.

- [ ] **컴포넌트를 분리/조합**하고, `children`을 활용해 레이아웃 컴포넌트를 만들 수 있다.
- [ ] **조건부 렌더링**과 **리스트 렌더링**으로 다양한 화면을 구성할 수 있다.
- [ ] **CSS Module**과 **Tailwind CSS**의 차이를 알고, 같은 컴포넌트를 두 방식으로 스타일링할 수 있다.
- [ ] **Context API**로 컴포넌트 트리 전체에 데이터를 공유할 수 있다 (다크모드 토글 예제).

---

## 1. React 컴포넌트 심화

### 1.1 컴포넌트를 분리하는 기준

"언제 컴포넌트를 나눌까?" 자주 쓰는 기준:

- **반복되는 UI** (예: 리스트 아이템)
- **독립적인 책임** (예: `Header`, `Sidebar`, `Footer`)
- **너무 커진 컴포넌트** (보통 100줄 넘어가면 분리 고려)

```tsx
// ❌ 모든 걸 한 컴포넌트에
function App() {
  return (
    <div>
      <header><h1>My App</h1></header>
      <main>
        {users.map(u => <div className="card"><img/><h3>{u.name}</h3></div>)}
      </main>
    </div>
  )
}

// ✅ 역할별로 분리
function Header() {
  return <header><h1>My App</h1></header>
}

interface UserCardProps {
  name: string
  avatar: string
}

function UserCard({ name, avatar }: UserCardProps) {
  return (
    <div className="card">
      <img src={avatar} alt={name} />
      <h3>{name}</h3>
    </div>
  )
}

interface UserListProps {
  users: UserCardProps[]
}

function UserList({ users }: UserListProps) {
  return (
    <main>
      {users.map(u => <UserCard key={u.name} {...u} />)}
    </main>
  )
}

function App() {
  const users: UserCardProps[] = [
    { name: '영웅', avatar: '/a.png' },
    { name: '민수', avatar: '/b.png' },
  ]
  return (
    <div>
      <Header />
      <UserList users={users} />
    </div>
  )
}
```

### 1.2 `children` — "컴포넌트에 콘텐츠를 끼워넣기"

HTML의 `<div>아무내용</div>`처럼, 컴포넌트에도 `children`을 전달할 수 있습니다.

```tsx
import type { ReactNode } from 'react'

interface CardProps {
  title: string
  children: ReactNode   // 컴포넌트 안에 들어올 JSX
}

function Card({ title, children }: CardProps) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-body">{children}</div>
    </div>
  )
}

// 사용
function App() {
  return (
    <Card title="공지사항">
      <p>새로운 기능이 추가되었습니다.</p>
      <button>확인</button>
    </Card>
  )
}
```

> 💡 `children`은 React에서 가장 흔한 합성 패턴입니다. 모달, 사이드바, 카드 같은 **"컨테이너"** 컴포넌트는 거의 다 `children`을 받습니다.

#### "요소" vs "슬롯" 용어

강의에서 헷갈리기 쉬운 두 단어를 정리합니다.

| 용어 | 의미 | 예시 |
| --- | --- | --- |
| **요소 (Element)** | JSX로 만든 **하나의 블록** (HTML 태그, 컴포넌트) | `<form>`, `<TextField />`, `<MyButton>확인</MyButton>` |
| **슬롯 (Slot)** | 부모가 **자식 컴포넌트에 끼워 넣을 자리** | `<Card>...</Card>`의 `...` 부분 = `children` |

다시 말해, `<Card>...</Card>`를 쓸 때 `<Card>`는 컴포넌트(요소의 한 종류)이고, `<Card>`와 `</Card>` 사이에 들어가는 모든 JSX가 **슬롯(= `children`)**입니다.

```tsx
// Card는 컴포넌트 (= 요소)
// "공지사항 내용" 부분이 슬롯 (= children)
<Card title="공지">
  <p>슬롯 1번</p>
  <button>슬롯 2번</button>
</Card>
```

### 1.3 조건부 렌더링 — "상황에 따라 다른 화면"

```tsx
function Greeting({ isLogin, name }: { isLogin: boolean; name: string }) {
  // 방법 1) && (조건이 참일 때만 렌더)
  return (
    <div>
      {isLogin && <p>환영합니다, {name}님!</p>}
      {!isLogin && <p>로그인이 필요합니다.</p>}
    </div>
  )
}

function Status({ status }: { status: 'idle' | 'loading' | 'error' | 'success' }) {
  // 방법 2) switch / 객체 맵
  const messages = {
    idle: '대기 중',
    loading: '불러오는 중...',
    error: '에러 발생',
    success: '완료!',
  }
  return <p>{messages[status]}</p>
}
```

### 1.4 리스트 렌더링 — "배열을 화면으로"

배열(`Array`)은 Day 1에서 잠깐 봤죠. (참고: `../javascript/08-배열`)  
React에서는 배열을 `map`으로 돌려서 JSX 배열을 만듭니다.

```tsx
interface Todo {
  id: number
  title: string
  done: boolean
}

function TodoList({ todos }: { todos: Todo[] }) {
  if (todos.length === 0) return <p>할 일이 없어요 🎉</p>

  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id} className={todo.done ? 'done' : ''}>
          <input type="checkbox" checked={todo.done} readOnly />
          <span>{todo.title}</span>
        </li>
      ))}
    </ul>
  )
}
```

> ⚠️ **반드시 `key`를 줘야 합니다.** React가 어떤 항목이 바뀌었는지 구분할 때 사용합니다. 보통 데이터의 고유 ID(`id`, `uuid`)를 씁니다.

---

## 2. CSS Module vs Tailwind CSS

### 2.1 세 가지 스타일링 방법 비교

| 방식 | 장점 | 단점 |
| --- | --- | --- |
| **일반 CSS** (`App.css`) | 익숙함 | 클래스 이름 충돌 위험 |
| **CSS Module** (`App.module.css`) | 클래스 자동 스코프 (안전) | 별도 파일 작성 |
| **Tailwind CSS** | 클래스 이름 짓기 고민 ↓, 반응형 쉬움 | HTML이 길어 보임, 학습 필요 |

### 2.2 CSS Module — "파일 단위로 스타일이 격리됨"

Vite + React + TS 템플릿은 CSS Module을 기본 지원합니다.

**1) 파일 만들기: `Button.module.css`**

```css
/* Button.module.css */
.button {
  background: #0374ff;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 6px;
  cursor: pointer;
}

.button:hover {
  background: #0256bf;
}
```

**2) 컴포넌트에서 사용: `Button.tsx`**

```tsx
import styles from './Button.module.css'

interface ButtonProps {
  children: React.ReactNode
  onClick?: () => void
}

export default function Button({ children, onClick }: ButtonProps) {
  return (
    <button className={styles.button} onClick={onClick}>
      {children}
    </button>
  )
}
```

핵심:

- `styles.button`처럼 **객체**로 접근합니다.
- 클래스 이름은 빌드 시 자동으로 고유한 이름(예: `_button_a1b2c`)으로 바뀌어, 다른 파일과 충돌하지 않습니다.

### 2.3 Tailwind CSS — "HTML 안에서 스타일을 끝내기"

Tailwind는 **유틸리티 클래스**를 조합해서 스타일을 만드는 방식입니다. (참고: [Tailwind 핵심 패턴](https://www.heropy.dev/p/E67ZHS))

**1) Tailwind 설치**

```bash
npm install -D tailwindcss @tailwindcss/vite
```

**2) `vite.config.ts`에 플러그인 추가**

```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```

**3) `src/index.css`에 한 줄**

```css
@import "tailwindcss";
```

**4) 같은 Button을 Tailwind로**

```tsx
interface ButtonProps {
  children: React.ReactNode
  onClick?: () => void
}

export default function Button({ children, onClick }: ButtonProps) {
  return (
    <button
      onClick={onClick}
      className="bg-blue-500 hover:bg-blue-700 text-white px-4 py-2 rounded-md transition-colors"
    >
      {children}
    </button>
  )
}
```

> 💡 처음엔 "이게 무슨 CSS지?" 싶지만, **클래스 이름이 곧 스타일**이라 별도 CSS 파일을 오가지 않아도 됩니다.

### 2.4 자주 쓰는 Tailwind 클래스 모음

| 의도 | Tailwind 클래스 | 의미 |
| --- | --- | --- |
| 배경색 | `bg-blue-500`, `bg-white`, `bg-black/50` | 파란색 배경, 반투명 검정 |
| 글자색 | `text-white`, `text-gray-700` | 흰색 글자, 회색 글자 |
| 여백 | `p-4` (16px), `m-2` (8px), `px-3` (좌우), `py-2` (위아래) | 패딩/마진 |
| 크기 | `w-32`, `h-10`, `w-full`, `h-screen` | 너비/높이 |
| 모서리 | `rounded`, `rounded-lg`, `rounded-full` | 둥근 모서리 |
| 정렬 | `flex`, `items-center`, `justify-between` | Flexbox |
| 글자 크기 | `text-sm`, `text-lg`, `text-2xl`, `font-bold` | 14px, 18px, 24px, 굵게 |
| 반응형 | `md:w-1/2`, `lg:flex-row` | 태블릿/데스크탑에서 변경 |

**예제: 카드 한 장**

```tsx
<div className="max-w-sm mx-auto bg-white rounded-lg shadow-md p-6 hover:shadow-lg transition-shadow">
  <h2 className="text-xl font-bold mb-2">카드 제목</h2>
  <p className="text-gray-700">카드에 들어갈 내용...</p>
</div>
```

> 처음엔 외울 필요 없고, [Tailwind 치트시트](https://tailwindcss.com/docs/utility-first)를 옆에 띄워두고 자주 쓰는 것만 먼저 익히면 됩니다.

---

## 3. Context API — "트리 전체에 데이터 공유"

### 3.1 왜 필요한가?

Day 1의 `Counter`처럼 state는 **해당 컴포넌트 안에서만** 살아 있습니다.  
만약 `App` → `Header` → `UserMenu` → `Avatar` 처럼 깊숙한 곳까지 props로 데이터를 내려보내야 한다면?

```tsx
// ❌ Props drilling: 중간 컴포넌트가 사용하지도 않는 데이터를 들고 내려야 함
<App user={user}>
  <Header user={user}>
    <UserMenu user={user}>
      <Avatar user={user} />  {/* 진짜 쓰는 건 여기 */}
    </UserMenu>
  </Header>
</App>
```

**Context API**를 사용하면, 중간 컴포넌트들을 거치지 않고도 **트리 어디서든 데이터에 접근**할 수 있습니다.

### 3.2 Context API 3단계

```txt
1) createContext: "이런 데이터를 공유할 거야"라고 선언
2) Provider:    "이 데이터의 현재 값은 이거야"라고 트리 상위에 제공
3) useContext:  "데이터를 받을래!"라고 트리 하위에서 꺼내 씀
```

### 3.3 예제 — 다크모드 토글

**1) `src/contexts/ThemeContext.tsx` 생성**

```tsx
import { createContext, useContext, useState, type ReactNode } from 'react'

// 1) Context 생성 — 데이터 모양 정의
type Theme = 'light' | 'dark'
interface ThemeContextValue {
  theme: Theme
  toggle: () => void
}

const ThemeContext = createContext<ThemeContextValue | null>(null)

// 2) Provider 컴포넌트 — 데이터 공급
export function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setTheme] = useState<Theme>('light')

  const toggle = () =>
    setTheme(prev => (prev === 'light' ? 'dark' : 'light'))

  return (
    <ThemeContext.Provider value={{ theme, toggle }}>
      {children}
    </ThemeContext.Provider>
  )
}

// 3) 커스텀 훅 — 편하게 꺼내 쓰기
export function useTheme() {
  const ctx = useContext(ThemeContext)
  if (!ctx) throw new Error('ThemeProvider 안에서만 사용 가능합니다!')
  return ctx
}
```

**2) `src/main.tsx`에서 Provider로 감싸기**

```tsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import App from './App'
import { ThemeProvider } from './contexts/ThemeContext'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <ThemeProvider>
      <App />
    </ThemeProvider>
  </StrictMode>,
)
```

**3) 아무 컴포넌트에서나 사용**

```tsx
import { useTheme } from './contexts/ThemeContext'

function ThemeToggleButton() {
  const { theme, toggle } = useTheme()  // 🎉 props 없이 바로 접근!
  return (
    <button onClick={toggle} className="p-2 rounded border">
      현재: {theme} → 클릭해서 변경
    </button>
  )
}

function Page() {
  const { theme } = useTheme()
  return (
    <main className={theme === 'dark' ? 'bg-gray-900 text-white' : 'bg-white text-black'}>
      <ThemeToggleButton />
      <p>다크모드 예제입니다.</p>
    </main>
  )
}
```

### 3.4 Context 흐름 그림

```txt
            ┌────────────────────────┐
            │     ThemeProvider      │
            │  state: theme='light'  │
            │  value: { theme, toggle }
            └──────────┬─────────────┘
                       │ Context.Provider
            ┌──────────▼─────────────┐
            │          App           │
            └──────────┬─────────────┘
                       │
        ┌──────────────┴──────────────┐
        ▼                             ▼
   ┌─────────┐                  ┌─────────┐
   │  Page   │                  │ Header  │
   └────┬────┘                  └────┬────┘
        │ useTheme()                  │ useTheme()
        ▼                             ▼
   theme 적용                    ThemeToggleButton
```

> 💡 **언제 Context를 쓰는가?**
> - 사용자 인증 정보 (현재 로그인한 사용자)
> - 테마 (다크모드)
> - 언어 / 통화
> - 앱 전체에서 자주 바뀌는 설정
>
> 너무 많은 데이터를 한 Context에 넣으면 성능이 떨어질 수 있으니, **변경 빈도가 비슷한 것끼리** 묶는 게 좋습니다.

#### `import` / `export` 두 가지 방식

`18-모듈`에서는 named export만 봤지만, React 강의 코드는 **default export**를 많이 씁니다. 둘을 명확히 구분하세요.

```ts
// ========== export 쪽 (정의하는 파일) ==========

// 1) default export — 파일당 1개, 이름이 강제되지 않음
export default function Button(props) { ... }
export default function () { ... }   // 익명도 OK (이름 없어도 됨)

// 2) named export — 여러 개, 반드시 이름 필요
export const age = 20
export const isValid = false
export function add(a, b) { return a + b }

// ========== import 쪽 (가져오는 파일) ==========

// 1) default 가져오기 — 이름 자유
import Button from './Button'           // 아무 이름이나 OK
import Btn from './Button'              // 이렇게 바꿔 써도 됨

// 2) named 가져오기 — 이름이 일치해야 함
import { age, isValid } from './data'
import { add as plus } from './data'    // as로 이름 바꾸기 가능

// 3) 섞어서
import Button, { age, isValid } from './data'  // default + named 동시
import * as data from './data'                  // 전체를 객체로
// data.age, data.isValid 로 접근
```

| 구분 | 개수 | 이름 | 사용 |
| --- | --- | --- | --- |
| `export default` | 파일당 1개 | 자유 | `import 아무거나 from '...'` |
| `export` (named) | 여러 개 | 필수 | `import { 이름 } from '...'` |

> Day 1~2 강의의 모든 컴포넌트(`App`, `Counter`, `Button`, `TextField`, `Header` 등)는 `export default function ...` 형태입니다.

---

## 4. 실습 — Todo 앱의 셸 만들기

Day 4~5에서 만들 Todo 앱의 뼈대를 미리 만들어 봅니다.

**폴더 구조**

```txt
src/
├── components/
│   ├── Button.tsx
│   └── TodoItem.tsx
├── contexts/
│   └── ThemeContext.tsx
├── App.tsx
└── main.tsx
```

**`src/components/Button.tsx`** — Tailwind로 작성한 재사용 버튼

```tsx
import type { ButtonHTMLAttributes, ReactNode } from 'react'

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  children: ReactNode
  variant?: 'primary' | 'ghost'
}

export default function Button({
  children,
  variant = 'primary',
  className = '',
  ...rest
}: ButtonProps) {
  const base = 'px-3 py-2 rounded-md transition-colors text-sm'
  const styles =
    variant === 'primary'
      ? 'bg-blue-500 text-white hover:bg-blue-700'
      : 'bg-transparent text-gray-700 hover:bg-gray-100'

  return (
    <button className={`${base} ${styles} ${className}`} {...rest}>
      {children}
    </button>
  )
}
```

> ✨ `...rest`와 `ButtonHTMLAttributes<HTMLButtonElement>` 덕분에 `onClick`, `type` 등 모든 표준 버튼 속성을 그대로 전달할 수 있습니다.

#### `extends ButtonHTMLAttributes<HTMLButtonElement>` 패턴 해부

이 한 줄이 왜 중요한지 분해해 봅니다.

```ts
import type { ButtonHTMLAttributes, ReactNode } from 'react'

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  // ↑ 표준 <button>이 받을 수 있는 모든 속성(onClick, type, disabled, aria-*, ...)을
  //   자동으로 ButtonProps에 "물려받음" — extends(상속) 키워드
  children: ReactNode    // ← 표준에는 없는 우리만의 추가 속성
  variant?: 'primary' | 'ghost'
}
```

그래서 `Button`을 쓸 때 `type="submit"`, `disabled`, `aria-label` 같은 표준 속성도 **그대로 타입 안전**하게 쓸 수 있습니다.

```tsx
<Button type="submit" disabled={loading} aria-label="로그인">
  로그인
</Button>
```

> 비슷한 패턴이 매우 자주 등장합니다.
> ```ts
> interface InputProps extends InputHTMLAttributes<HTMLInputElement> { ... }
> interface DivProps   extends HTMLAttributes<HTMLDivElement> { ... }
> ```
> "표준 HTML 속성 + 우리 프로젝트 고유 속성"을 합치는 표준 방식입니다.

---

**`src/components/TodoItem.tsx`** — 한 줄 할 일

```tsx
interface Todo {
  id: number
  title: string
  done: boolean
}

interface TodoItemProps {
  todo: Todo
  onToggle: (id: number) => void
}

export default function TodoItem({ todo, onToggle }: TodoItemProps) {
  return (
    <li className="flex items-center gap-2 p-2 border-b">
      <input
        type="checkbox"
        checked={todo.done}
        onChange={() => onToggle(todo.id)}
      />
      <span className={todo.done ? 'line-through text-gray-400' : ''}>
        {todo.title}
      </span>
    </li>
  )
}
```

**`src/App.tsx`**

```tsx
import { useState } from 'react'
import Button from './components/Button'
import TodoItem from './components/TodoItem'

interface Todo {
  id: number
  title: string
  done: boolean
}

function App() {
  const [todos, setTodos] = useState<Todo[]>([
    { id: 1, title: 'TypeScript 기본 문법 읽기', done: true },
    { id: 2, title: 'Counter 컴포넌트 만들기', done: false },
  ])

  const toggle = (id: number) => {
    setTodos(prev =>
      prev.map(t => (t.id === id ? { ...t, done: !t.done } : t))
    )
  }

  return (
    <div className="max-w-md mx-auto p-4">
      <h1 className="text-2xl font-bold mb-4">My Todo</h1>
      <ul className="border rounded">
        {todos.map(todo => (
          <TodoItem key={todo.id} todo={todo} onToggle={toggle} />
        ))}
      </ul>
      <div className="mt-4">
        <Button onClick={() => alert('Day 3에서 입력 폼을 만들 거예요!')}>
          + 새 할 일
        </Button>
      </div>
    </div>
  )
}

export default App
```

> 이 코드에서 사용한 것: 컴포넌트 분리, Props 타입, 배열 `map`, 조건부 클래스, Tailwind 유틸리티 클래스.  
> Day 3에서는 이 상태 관리 방식을 **Zustand**로 바꿔 봅니다.

---

## 5. 자주 하는 실수 체크리스트

- [ ] `children`을 받을 때 `ReactNode` 타입을 안 씀 (JSX 못 넣음)
- [ ] CSS Module 클래스 접근을 `className="button"`으로 함 → `styles.button`이어야 함
- [ ] Tailwind 클래스 오타 (`bg-blue-50` ❌ → `bg-blue-500`이 맞음)
- [ ] `key`로 `index`만 사용 (리스트에 아이템 추가/삭제가 잦으면 깨질 수 있음)
- [ ] Context Provider로 감싸지 않고 `useContext` 사용 → 값이 `null`

---

## 6. 연습 문제

### 문제 1. `Card` 컴포넌트

`title: string`과 `children: ReactNode`을 받아 카드 모양으로 감싸는 `Card` 컴포넌트를 만들어 보세요.  
스타일은 Tailwind로 (`border`, `rounded-lg`, `p-4`, `shadow`).

### 문제 2. 빈 리스트 안내

`UserList` 컴포넌트가 `users: User[]` Props를 받을 때, 배열이 비어 있으면 `"사용자가 없습니다."`를, 아니면 리스트를 보여주는 코드를 작성해 보세요. (조건부 렌더링)

### 문제 3. Locale Context

`locale: 'ko' | 'en'` 상태를 갖는 `LocaleContext`를 만들고, `useLocale()` 훅으로 현재 locale과 `setLocale`을 꺼내 쓰는 컴포넌트를 작성해 보세요.

---

## 7. 요약

| 개념 | 한 줄 |
| --- | --- |
| **컴포넌트 분리** | 역할/반복/크기 기준으로 쪼갠다 |
| **`children`** | 컴포넌트 안에 JSX를 끼워넣는 표준 패턴 |
| **조건부 렌더링** | `&&` 또는 삼항 연산자, 객체 맵 활용 |
| **리스트 렌더링** | `array.map` + 안정적인 `key` |
| **CSS Module** | 파일 단위로 스타일이 격리됨 (`styles.클래스명`) |
| **Tailwind CSS** | HTML 안에서 유틸리티 클래스로 스타일링 |
| **Context API** | 트리 전체에서 props 없이 데이터 공유 |

---

## 8. 다음 단계

Day 3에서는 컴포넌트 안의 state를 **Zustand**라는 외부 저장소로 옮기고, **React Router**로 여러 페이지를 만들어 봅니다.

👉 [Day 3 노트 열기 — Zustand + React Router 기초](./../day3/README.md)