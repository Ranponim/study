# Day 1. 프로젝트 구성 + TypeScript 기초 + React 기본 문법

## 학습 목표

Day 1이 끝나면 다음을 할 수 있습니다.

- [ ] **Vite**로 React + TypeScript 프로젝트를 새로 만들 수 있다.
- [ ] **TypeScript 기본 타입** (`string`, `number`, `boolean`, `array`, `object`, `union`, `interface`)을 읽고 쓸 수 있다.
- [ ] **JSX**가 무엇인지 설명할 수 있다.
- [ ] **함수형 컴포넌트**와 **Props**를 이해하고 간단한 컴포넌트를 만들 수 있다.
- [ ] **이벤트** (`onClick`)와 **상태** (`useState`)로 숫자 증가 버튼을 만들 수 있다.

---

## 0. 들어가기 전에 — "왜 React + TypeScript 인가?"

### React는 무엇인가?

리액트(React)는 **"사용자 인터페이스(UI)를 조각내서 만들고, 상태가 바뀌면 자동으로 다시 그려주는"** 라이브러리입니다.

기존 JavaScript만 사용했을 때는 `document.querySelector(...).innerHTML = ...` 같은 방식으로 DOM을 직접 조작했습니다. (참고: `../javascript/12-DOM기초`)  
하지만 앱이 커지면 "어떤 상태가 바뀌면 어떤 DOM을 바꿔야 하지?"를 사람이 직접 관리하기 어려워집니다.

**React의 핵심 아이디어 3가지**

| 개념 | 한 줄 설명 | 비유 |
| --- | --- | --- |
| **컴포넌트** | UI를 독립된 조각(`Component`)으로 나눈다. | 레고 블록 |
| **상태(State)** | 변하는 데이터를 컴포넌트가 기억한다. | 블록 안의 "현재 숫자" |
| **재렌더링** | 상태가 바뀌면 React가 화면을 알아서 다시 그린다. | 자동 리모컨 |

### TypeScript는 무엇인가?

기존 JavaScript는 변수에 무엇이든 담을 수 있어서, 실행해 보기 전에는 오류를 알기 어렵습니다.

```js
// JavaScript
function add(a, b) {
  return a + b;
}
add("3", 5); // "35" ← 사람이 의도하지 않은 결과!
```

**TypeScript**는 JavaScript에 **"타입(자료형)"을 미리 적어두는** 문법을 추가한 것입니다.  
에디터에서 코드를 작성하는 순간(컴파일 타임)에 잘못된 부분을 잡아줍니다.

```ts
// TypeScript
function add(a: number, b: number): number {
  return a + b;
}
add("3", 5); // ❌ 에러! "3"은 number가 아니야!
```

> 즉, **TypeScript = JavaScript + 타입 검사**입니다. 실행 전 안전벨트를 두르는 것과 같습니다.

---

## 1. 프로젝트 구성 (Vite + React + TS)

> npm 설치와 기본 사용법은 알고 있다고 가정합니다. (참고: `../javascript/19-npm맛보기`)

### 1.1 프로젝트 생성

원하는 위치에서 다음 한 줄을 실행합니다.

```bash
npm create vite@latest my-app -- --template react-ts
```

- `my-app` 폴더가 새로 만들어지고, 그 안에 React + TypeScript 보일러플레이트가 들어 있습니다.
- `--template react-ts`는 "React + TypeScript" 템플릿을 의미합니다. (JS만 쓸 때는 `react`)

### 1.2 설치 및 실행

```bash
cd my-app
npm install   # node_modules 폴더 생성
npm run dev   # 개발 서버 실행 (보통 http://localhost:5173)
```

> ⚠️ `npm install`이 끝나면 `node_modules` 폴더가 생깁니다. 이 폴더는 `.gitignore`에 등록되어 있어야 git에 올라가지 않습니다. (이미 루트 `.gitignore`에 포함되어 있습니다.)

### 1.3 폴더 구조 (생성 직후)

```txt
my-app/
├── node_modules/        ← git에 올라가지 않음
├── public/              ← 정적 파일 (이미지 등)
├── src/
│   ├── App.tsx          ← 메인 컴포넌트
│   ├── main.tsx         ← 진입점 (HTML에 App을 마운트)
│   └── assets/
├── index.html           ← 브라우저가 여는 파일
├── package.json         ← 프로젝트 정보 + 스크립트
├── tsconfig.json        ← TypeScript 설정
└── vite.config.ts       ← Vite 설정
```

핵심 3개 파일을 외워두면 됩니다.

- **`index.html`** — 브라우저가 처음 여는 HTML (대부분 `<div id="root"></div>`만 있음)
- **`src/main.tsx`** — React를 실제로 DOM에 붙이는 코드
- **`src/App.tsx`** — 우리가 만들 화면의 "뿌리" 컴포넌트

### 1.4 main.tsx 살펴보기

```tsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import App from './App'
import './index.css'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

읽을 줄만 알면 됩니다.

- `createRoot(...).render(<App />)` → HTML의 `#root` 요소에 `<App />` 컴포넌트를 그린다.
- `<StrictMode>` → React가 잠재적 문제를 미리 잡아주는 "개발용 안전 장치".

---

## 2. TypeScript 기초

TypeScript는 "JavaScript에 타입을 붙인 것"이므로, **JS에서 이미 아는 것 + 타입 표기**만 추가로 배우면 됩니다.

### 2.1 원시 타입

```ts
// 변수명: 타입 = 값
let userName: string = '영웅'
let age: number = 30
let isActive: boolean = true

// 타입 추론(Type Inference): 값을 넣으면 타입을 자동으로 잡아줍니다.
let city = 'Seoul'   // city는 자동으로 string
city = 123            // ❌ Error! string 변수에는 number를 못 넣음
```

> 💡 **실무 팁**: 타입을 매번 적기보다 **타입 추론을 믿고, 함수 매개변수처럼 추론이 안 되는 곳에서만 타입을 표기**하는 게 일반적입니다.

#### 원시 타입 vs 참조 타입 — "왜 React 상태는 항상 새로 만들어야 할까?"

JavaScript의 값은 두 부류로 나뉩니다.

| 종류 | 값 | 비교 | 복사 |
| --- | --- | --- | --- |
| **원시(Primitive)** | `string`, `number`, `boolean`, `null`, `undefined`, `symbol`, `bigint` | **값**으로 비교 (`1 === 1` true) | 진짜로 복사됨 |
| **참조(Reference)** | `array`, `object`, `function` | **참조**(주소)로 비교 (`{a:1} === {a:1}` false) | 참조가 복사됨 |

```ts
const a = [1, 2]
const b = a          // 같은 배열을 가리킴 (복사 X)
b.push(3)
console.log(a)       // [1, 2, 3] — a도 바뀜!

const updated = [...a, 4]   // 새 배열을 만들어야 독립
```

> 💡 그래서 React에서 상태를 바꿀 때 `{ ...user, age: 21 }` 처럼 **spread**로 새 객체를 만들어 전달합니다. 원본을 직접 바꾸면 React가 변화를 감지 못 합니다. 22~26강에서 자세히 다룹니다.

### 2.2 배열과 튜플

```ts
// 배열: 같은 타입의 모음
const numbers: number[] = [1, 2, 3]
const names: Array<string> = ['a', 'b']

// 여러 타입을 섞고 싶을 때: union
const mixed: (string | number)[] = ['a', 1, 'b', 2]

// 튜플: 길이가 고정된 배열
const tuple: [string, number] = ['age', 30]
```

### 2.3 객체와 interface

JS에서 객체를 배웠죠. (참고: `../javascript/09-객체`)  
TS에서는 객체의 **모양(shape)**을 미리 정의해 둘 수 있습니다.

```ts
// interface: "이런 모양의 객체야"라고 선언
interface User {
  name: string
  age: number
  isAdmin?: boolean   // ?는 있어도 되고 없어도 된다는 의미 (선택적)
}

const me: User = {
  name: '영웅',
  age: 30,
}

const you: User = {
  name: '민수',
  age: 25,
  isAdmin: true,      // 있어도 OK
}
```

> **왜 interface를 쓰는가?**  
> - 에디터 자동완성이 좋아집니다.  
> - 객체의 키 이름을 오타 내면 바로 에러로 잡힙니다.  
> - "이 함수에는 이런 모양의 객체가 들어와야 해"라고 명확히 표현할 수 있습니다.

### 2.4 함수 타입

```ts
// 매개변수와 반환값에 타입을 붙일 수 있음
function add(a: number, b: number): number {
  return a + b
}

// 반환값이 없을 때는 void
function log(message: string): void {
  console.log(message)
}

// 화살표 함수 + 타입
const multiply = (a: number, b: number): number => a * b
```

### 2.5 Union 타입과 Type Alias

```ts
// union: "여러 타입 중 하나"
type Status = 'idle' | 'loading' | 'success' | 'error'
let s: Status = 'loading'  // OK
// s = 'done'             // ❌ Error! 미리 정의한 값만 허용

// type alias: 타입에 이름 붙이기
type ID = string | number
```

### 2.6 자주 보는 컴파일 에러 패턴

| 메시지 | 의미 | 해결 |
| --- | --- | --- |
| `Type 'string' is not assignable to type 'number'` | string을 number 변수에 넣음 | 타입을 맞추거나 캐스팅 |
| `Property 'X' does not exist on type 'Y'` | 객체에 없는 속성에 접근 | 인터페이스에 속성 추가 또는 존재 확인 |
| `Argument of type '...' is not assignable to parameter of type '...'` | 함수 인자 타입 불일치 | 시그니처 확인 |

> ⚠️ **TypeScript는 친절한 선생님**입니다. 빨간 줄이 떠도 당황하지 말고, 마우스를 올려서 메시지를 읽으면 답이 거의 다 나와 있습니다.

---

## 3. React 기본 문법

### 3.1 JSX — "HTML 같은 문법, 하지만 JS"

```tsx
// JSX: JavaScript 안에서 HTML 태그처럼 쓰는 문법
const element = <h1>안녕하세요!</h1>

// 변수/함수를 {}로 감싸서 끼워 넣을 수 있음
const name = '영웅'
const greeting = <h1>안녕하세요, {name}님!</h1>
```

JSX 규칙 (외우기):

1. **하나의 부모로 감싸야 한다.** (`<div>...</div>` 또는 `<>...</>`)
2. `class` → `className`, `for` → `htmlFor` (HTML 속성 이름과 일부 다름)
3. 닫는 태그가 필요한 태그는 꼭 닫기 (`<br />`, `<img />`)
4. JS 표현식은 `{}`로 감싼다.

```tsx
// 예: 조건부 렌더링
const isLogin = true
return (
  <div>
    {isLogin ? <p>환영합니다!</p> : <p>로그인이 필요합니다.</p>}
  </div>
)

// 예: 리스트 렌더링 (반복문)
const fruits = ['사과', '바나나', '포도']
return (
  <ul>
    {fruits.map((fruit, i) => (
      <li key={i}>{fruit}</li>
    ))}
  </ul>
)
```

> 💡 `key`는 React가 어떤 항목이 바뀌었는지 구분할 때 쓰는 "이름표"입니다. 지금은 배열의 인덱스를 넣어도 동작하지만, 실무에서는 안정적인 ID를 쓰는 게 좋습니다.

#### 보간법(Interpolation)의 한계 — "뭘 넣을 수 있나?"

`{}` 안에는 **JS 표현식**이 들어갑니다. 결국 화면에 출력하려면 React가 **문자열/숫자**로 바꿀 수 있어야 합니다.

```tsx
// ✅ OK — 숫자, 문자열, 계산 결과, 삼항
<p>{1 + 2}</p>                // 3
<p>{`안녕, ${name}님`}</p>    // 안녕, 영웅님
<p>{isLogin ? '환영' : '로그인'}</p>

// ❌ 에러/경고 — 객체, 배열, 불리언을 그대로 넣을 수 없음
<p>{true}</p>                  // 안 보임 (불리언은 React가 무시)
<p>{user}</p>                  // 에러! 객체는 React가 출력 못 함
<p>{a: 123}</p>                // 문법 에러
```

해결: 객체는 `.`로 풀어 쓰고, 배열은 `join()`을 씁니다.

```tsx
<p>{user.name}</p>
<p>{fruits.join(', ')}</p>
```

### 3.2 컴포넌트 — "UI를 함수로 만드는 것"

**함수형 컴포넌트**가 표준입니다. JS의 함수 (참고: `../javascript/07-함수`)에서 **JSX를 반환**하면 컴포넌트입니다.

```tsx
// 1) 가장 기본적인 컴포넌트
function Hello() {
  return <h1>Hello, React!</h1>
}

// 2) 다른 파일에서 불러와서 사용
// export default function Hello() { ... }

// 사용: HTML 태그처럼 사용 (대문자로 시작!)
function App() {
  return (
    <div>
      <Hello />
      <Hello />
    </div>
  )
}
```

> ⚠️ 컴포넌트 이름은 반드시 **대문자**로 시작합니다. (`<hello />` 처럼 소문자면 React는 그냥 HTML 태그로 인식합니다.)

### 3.3 Props — "부모가 자식에게 데이터를 전달"

```tsx
// 자식: name을 props로 받음
interface HelloProps {
  name: string
  age?: number  // 선택적
}

function Hello({ name, age = 20 }: HelloProps) {
  return (
    <p>
      안녕하세요, {name}님! ({age}살)
    </p>
  )
}

// 부모: name을 전달
function App() {
  return (
    <div>
      <Hello name="영웅" age={30} />
      <Hello name="민수" />          {/* age 없음 → 기본값 20 */}
    </div>
  )
}
```

핵심:

- **Props는 읽기 전용**입니다. 자식이 직접 바꿀 수 없습니다.
- 부모가 데이터를 바꾸면 자식도 자동으로 다시 그려집니다.

### 3.4 State — "컴포넌트가 기억하는 값"

화면이 바뀌어야 한다면 → **state**를 사용합니다.

```tsx
import { useState } from 'react'

function Counter() {
  // [현재 값, 값을 바꾸는 함수] = useState(초기값)
  const [count, setCount] = useState(0)

  return (
    <div>
      <p>현재 카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setCount(0)}>초기화</button>
    </div>
  )
}
```

흐름 정리:

```txt
1) 처음: count = 0, 화면 "현재 카운트: 0"
2) 버튼 클릭 → setCount(0 + 1) 호출
3) React가 count를 1로 업데이트
4) 컴포넌트가 "자동으로 다시 실행"됨
5) 화면이 "현재 카운트: 1"로 바뀜
```

> 🚨 절대 하면 안 되는 것:  
> ```tsx
> count = count + 1  // ❌ 직접 변경 → React가 모름
> setCount(count + 1) // ✅ 함수로만 변경
> ```

#### 단방향 데이터 흐름 — "React는 one-way binding"

Vue/Angular에는 `v-model`, `ngModel` 같은 **양방향 바인딩(two-way data binding)**이 있어, 입력값을 바꾸면 변수가 자동으로 바뀌고 그 반대도 됩니다.

React는 **단방향**입니다. 직접 양방향처럼 보이게 하려면 `value`(읽기) + `onChange`(쓰기) **두 가지**를 명시적으로 연결합니다.

```tsx
// React식 "양방향 흉내" — 사실은 한 방향이 value, 한 방향이 onChange
<input
  value={name}                    // state → input (읽기)
  onChange={e => setName(e.target.value)}  // input → state (쓰기)
/>
```

> 💡 처음 보면 두 개를 적어야 해서 번거롭지만, **데이터가 어디서 왔는지 항상 명확**하다는 장점이 있습니다. 강사가 적어둔 "2way data binding"은 이런 패턴을 말합니다.

### 3.5 이벤트 — "사용자 행동에 반응"

JS의 `addEventListener`와 비슷한데, JSX에서는 **`on이벤트명={함수}`** 형태로 작성합니다. (참고: `../javascript/13-이벤트`)

```tsx
function Form() {
  const [text, setText] = useState('')

  return (
    <div>
      <input
        value={text}
        onChange={(e) => setText(e.target.value)}  // 타이핑할 때마다 state 업데이트
      />
      <p>입력한 값: {text}</p>
    </div>
  )
}
```

> 💡 React에서는 HTML의 `onchange`가 아니라 **`onChange`** (카멜케이스)입니다.

#### 한글 입력 + Enter 키 — IME 처리

`<input>`에서 한글을 조합 중(예: "한"을 만들기 위해 `ㅎ` + `ㅏ` + `ㄴ` 입력 중)에 Enter를 누르면 **조합 완료** 신호가 먼저 발생합니다. 이 상태에서 `onKeyDown`이 동작하면 의도하지 않게 폼이 제출되는 버그가 생깁니다.

```tsx
onKeyDown={event => {
  // 조합 중(IME converting)에는 Enter를 무시
  if (event.nativeEvent.isComposing) return
  if (event.key === 'Enter') {
    addFruit()
  }
}}
```

> `event.nativeEvent.isComposing`은 한국어/중국어/일본어 입력에서 매우 자주 쓰는 패턴입니다. 강사가 "HTMLInputElement 사용 원리"와 함께 짚어준 부분입니다.

#### 입력 직접 참조하기 — `useRef<HTMLInputElement>`

`state`로 입력값을 다루면 **모든 타이핑마다 리렌더링**됩니다. 진짜 DOM 노드에 직접 접근하고 싶을 때 `useRef`를 씁니다.

```tsx
import { useRef, useState } from 'react'

function FruitInput() {
  const [fruits, setFruits] = useState<string[]>([])
  const [name, setName] = useState('')
  const inputRef = useRef<HTMLInputElement>(null)  // input 박스의 "주소"

  function addFruit() {
    setFruits([name, ...fruits])
    setName('')
    inputRef.current?.focus()  // 입력칸에 다시 포커스
  }

  return (
    <>
      <input
        ref={inputRef}        // 여기 박스의 진짜 DOM이 inputRef에 담김
        value={name}
        onChange={e => setName(e.target.value)}
        onKeyDown={e => {
          if (e.nativeEvent.isComposing) return
          if (e.key === 'Enter') addFruit()
        }}
      />
      <button onClick={addFruit}>추가</button>
      <div>{fruits.length}개</div>
      <ul>{fruits.map(f => <li key={f}>{f}</li>)}</ul>
    </>
  )
}
```

> `useRef<HTMLInputElement>(null)`의 `HTMLInputElement`는 "input 박스 전용 ref"라는 뜻의 **TypeScript 타입**입니다. 26강(타입 단언)에서 더 자세히 다룹니다.

---

## 4. 실습 — `Counter` 컴포넌트 만들기

지금까지 배운 것을 모두 써서, **`Counter`** 라는 컴포넌트를 만들어 봅니다.  
`src/App.tsx`의 내용을 아래처럼 통째로 교체해 보세요.

```tsx
import { useState } from 'react'
import './App.css'

interface CounterProps {
  start?: number
  step?: number
}

function Counter({ start = 0, step = 1 }: CounterProps) {
  const [count, setCount] = useState(start)

  return (
    <div className="counter">
      <h1>Counter</h1>
      <p>현재 값: <strong>{count}</strong></p>
      <button onClick={() => setCount(count + step)}>+{step}</button>
      <button onClick={() => setCount(count - step)}>-{step}</button>
      <button onClick={() => setCount(start)}>초기화</button>
    </div>
  )
}

function App() {
  return (
    <div>
      <Counter />
      <Counter start={10} step={5} />
    </div>
  )
}

export default App
```

여기서 사용한 것들 다시 확인:

- **TypeScript**: `interface CounterProps`, 기본값 `start = 0`
- **컴포넌트**: `Counter`, `App`
- **Props**: `<Counter start={10} step={5} />`
- **State**: `useState(start)`
- **이벤트**: `onClick={...}`
- **JSX**: `<div>`, `<button>`, `{}`로 값 끼워넣기

### 💻 개발 팁 — VS Code Emmet

JSX 안에서 **`div.counter>button.clicked*3`** 처럼 Emmet 약어를 쓰면 HTML을 빠르게 생성할 수 있습니다.

| 입력 | 확장 |
| --- | --- |
| `div.container>ul>li*3` | `<div class="container"><ul><li></li><li></li><li></li></ul></div>` |
| `button.bg-blue-500.hover:bg-blue-700` | 클래스 여러 개 한번에 |
| `form>(input+button)*2` | 괄호로 그룹화 |

> JSX에서는 class는 `className`으로 자동 매핑되는 경우가 많지만, Emmet 기본 약어의 `class`는 직접 바꿔야 할 때도 있습니다. Tab 키로 확정 직전 확인하세요.

---

## 5. 자주 하는 실수 체크리스트

- [ ] `useState`를 import 안 함 (`import { useState } from 'react'` 잊음)
- [ ] 컴포넌트 이름을 소문자로 시작함 (`function counter()` → `<counter />`로 호출하면 안 됨)
- [ ] `class` 대신 `className`을 안 씀 (`<div class="box">` ❌)
- [ ] 태그를 닫지 않음 (`<br>`, `<img>` → `<br />`, `<img />`)
- [ ] state를 직접 변경함 (`count = 1` ❌ → `setCount(1)` ✅)
- [ ] JSX 표현식에 객체를 직접 넣음 (`{user}` ❌ → `{user.name}` ✅)

---

## 6. 연습 문제

### 문제 1. 인사말 컴포넌트

`name`이라는 `string` Props를 받아서 `"안녕하세요, [name]님!"`을 표시하는 컴포넌트 `Greeting`을 만들어 보세요.  
`App`에서 `<Greeting name="영웅" />` 형태로 사용해 봅니다.

### 문제 2. 좋아요 버튼

`useState`로 `likes`라는 숫자를 관리하고, 버튼을 누르면 1씩 증가하는 `LikeButton` 컴포넌트를 만들어 보세요.  
`interface LikeButtonProps`로 `initialLikes?: number`를 정의하고 기본값은 0으로 설정합니다.

### 문제 3. 입력한 이름 표시

`<input>`에 입력한 값을 state로 저장하고, 그 아래에 "입력한 이름: ○○"로 실시간 표시하는 `NameInput` 컴포넌트를 만들어 보세요. (힌트: `value`, `onChange`)

---

## 7. 요약

| 개념 | 한 줄 |
| --- | --- |
| **Vite** | React 프로젝트를 빠르게 시작하는 빌드 도구 (`npm create vite@latest`) |
| **TypeScript** | JavaScript + 타입 표기 (`: string`, `interface User`) |
| **JSX** | JS 안에서 HTML처럼 쓰는 문법, `{}`로 표현식 끼워넣기 |
| **컴포넌트** | UI를 함수로 만든 것. 대문자로 시작. |
| **Props** | 부모 → 자식으로 데이터를 전달 (읽기 전용) |
| **State** | `useState`로 관리하는, 변할 수 있는 값. 변경 시 자동 재렌더링 |
| **이벤트** | `onClick`, `onChange` 등 `on이벤트명={함수}` 형태 |

---

## 8. 다음 단계

Day 1에서 만든 `Counter`를 발전시켜 봅니다.

👉 [Day 2 노트 열기 — React 컴포넌트 + Tailwind CSS + Context API](./../day2/README.md)