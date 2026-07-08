# 21강. 다음 단계 로드맵

## 목표

JavaScript 기초 과정을 마친 뒤 어떤 순서로 공부하면 좋은지 안내합니다.

## 1단계. JavaScript 기초 복습

먼저 02강부터 17강까지 다시 훑어보세요. 특히 아래 주제는 여러 번 반복해야 합니다.

- 변수와 자료형
- 조건문과 반복문
- 함수
- 배열과 객체
- DOM
- 이벤트
- 비동기와 fetch

## 1.5단계. React 대비 보충 (22~26강)

**React/TypeScript 강의를 듣기 전**에 22~26강을 한 번 더 들으세요. 이 5강은 React 코드에 **바로** 등장하는 문법만 골라 모았습니다.

| 강 | 주제 | 왜 필요한가 |
|---|---|---|
| [22-ES6-핵심-문법](../22-ES6-핵심-문법) | 구조분해, spread/rest, 템플릿 리터럴, 삼항 연산자, 화살표 함수 암묵적 반환 | `const [count, setCount] = useState(0)`, `set({ todos: [...s.todos, x] })` 같은 코드 이해 |
| [23-배열과-문자열-심화](../23-배열과-문자열-심화) | `find()`, `includes()`, `trim()`, `split()` | `todos.find((t) => t.id === id)`, `get().input.trim()` 이해 |
| [24-URL과-타이머](../24-URL과-타이머) | `encodeURIComponent`, `URLSearchParams`, `setTimeout` | 검색어 URL 처리, useEffect 디바운싱 패턴 이해 |
| [25-HTTP와-에러-처리](../25-HTTP와-에러-처리) | GET/POST/PUT/DELETE, 상태 코드, `try/catch`, `instanceof Error`, `response.ok` | Axios API 모듈, store의 `search`/`add`/`toggle`/`remove` 액션 이해 |
| [26-React-사전학습-정리](../26-React-사전학습-정리) | 옵셔널 체이닝, Truthy/Falsy, 콜백, `any`/`unknown`/`never`, `readonly`, 타입 좁히기, `as`/`!`, 제네릭 | 강사가 Day 3에 추천한 사전학습 노션을 우리 노트 스타일로 정리. `interface extends`, `useRef<HTMLInputElement>`, `event.nativeEvent.isComposing` 등 강의에서 등장한 패턴 포함 |

## 2단계. DOM 프로젝트 반복

새로운 개념을 계속 배우기보다 작은 프로젝트를 여러 번 만들어 보세요.

추천 프로젝트:

1. 카운터
2. 투두리스트
3. 계산기
4. 퀴즈 앱
5. 가계부
6. 날씨 앱

## 3단계. Git과 GitHub 익히기

코드를 저장하고 공유하려면 Git/GitHub가 필요합니다.

먼저 익힐 명령어:

```bash
git status
git add .
git commit -m "message"
git push
```

처음에는 명령어를 완벽히 이해하지 않아도 됩니다. 작은 프로젝트를 올리며 반복하면 익숙해집니다.

## 4단계. React 입문

DOM 조작에 익숙해졌다면 React를 배워도 좋습니다. React는 사용자 인터페이스를 컴포넌트 단위로 만드는 도구입니다.

React 전에 확인할 것:

- 함수가 익숙한가?
- 배열 `map()`을 사용할 수 있는가?
- 객체를 읽고 수정할 수 있는가?
- 이벤트를 이해하는가?

## 5단계. TypeScript는 나중에

TypeScript는 JavaScript에 타입을 더한 언어입니다. 초반부터 배우면 부담이 클 수 있습니다.

추천 순서:

```txt
JavaScript 기초 → DOM 프로젝트 → React 기초 → TypeScript 기초
```

## 6단계. 백엔드는 더 나중에

서버, 데이터베이스, 로그인 같은 백엔드 주제는 매우 중요하지만 처음부터 하면 어렵습니다.

추천 순서:

```txt
프론트엔드 기초 → fetch 이해 → 간단한 API 사용 → Node.js/Express 입문
```

## 포트폴리오 추천

GitHub에 올리기 좋은 초보 프로젝트:

1. 자기소개 페이지
2. 투두리스트
3. 계산기
4. 날씨 앱
5. 영화 검색 앱
6. 간단한 쇼핑 장바구니

## 마지막 조언

프로그래밍은 한 번에 이해하는 공부가 아닙니다. 같은 내용을 여러 번 만나면서 익숙해지는 공부입니다.

아래 반복을 계속하세요.

```txt
작게 만들기 → 실행하기 → 에러 읽기 → 고치기 → GitHub에 올리기
```

이 과정을 반복하면 실력이 쌓입니다.

## 처음으로 돌아가기

[전체 목차로 돌아가기](../README.md)
