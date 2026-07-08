# 26강. React 학습 전 보강 (강사 추천 사전학습 정리)

## 학습 목표

- `?.` 옵셔널 체이닝으로 안전하게 속성에 접근합니다.
- Truthy/Falsy를 이해해 조건식을 간결하게 씁니다.
- 콜백을 등록할 때 함수와 함수 호출을 구분합니다.
- TypeScript의 `any` / `unknown` / `never` 차이를 압니다.
- `interface extends` / `type` / `readonly` / `?` / 타입 좁히기 / 타입 단언 / 제네릭을 읽고 씁니다.

## 쉬운 설명

이 강의는 Day 3에서 강사가 추천한 [사전학습 노션](https://curse-battery-d1c.notion.site/React-JS-TS-38fc672eb95e8001b5e3de9510ca523d)을 우리 학습 노트 스타일로 정리한 것입니다. 22~25강에서 이미 다룬 **구조분해 / spread / 배열·문자열 메서드 / URL·타이머 / HTTP·에러**는 여기서 다시 다루지 않고, **22~25에 빠진 JS 핵심 패턴**과 **TypeScript 심화**만 모았습니다.

## 1. 불변성 vs 가변성 — `const`의 정확한 의미

`const`는 **재할당만** 막습니다. 객체/배열의 **내부 값 변경**은 막지 않습니다.

```js
const user = { name: "Kim" };
user.name = "Lee";      // 가능
// user = {};           // 에러! 재할당 불가

const nums = [3, 1, 2];
nums.push(3);           // 가능 (원본이 바뀜)
nums.sort();            // 가능 (원본이 바뀜)
const sorted = [...nums].sort();  // 복사본 정렬
```

> React에서 상태를 바꿀 때는 **반드시 새 객체/배열**을 만들어야 합니다. 원본을 직접 바꾸면 React가 변화를 감지 못 합니다.

## 2. 옵셔널 체이닝 `?.`

속성이 없을 수 있는 객체에 안전하게 접근합니다.

```js
const user = { profile: { name: "Kim" } };

user.profile.name;             // 'Kim'
user.account?.name;            // undefined (에러 X)
user.profile?.address?.city;   // 단계별로 안전 접근

user.getName?.();              // 메서드도 ?.로 호출 가능
list?.[0];                     // 배열도 ?. 접근 가능
```

> Day 2의 Context 코드에서 `if (!ctx) throw new Error(...)` 패턴이 등장합니다. 옵셔널 체이닝은 **읽기만** 할 때 쓰는 가벼운 대안입니다.

## 3. Truthy / Falsy

조건문에는 값 자체를 넣을 수 있습니다. **falsy**는 6개뿐입니다.

```js
// falsy 6개: false, 0, '', null, undefined, NaN
if (list.length) { /* 빈 배열이면 실행 X */ }
if (user)        { /* null/undefined면 실행 X */ }
```

비교는 항상 `===`로:

```js
"1" == 1;   // true (자동 변환 — 사용 금지)
"1" === 1;  // false (타입까지 비교 — 권장)
```

## 4. 콜백 등록 — `함수` vs `함수()`

이벤트 핸들러를 등록할 때 **괄호**를 붙이면 즉시 실행됩니다.

```js
button.addEventListener("click", handleClick);   // 클릭 시 실행
button.addEventListener("click", handleClick());  // 등록과 동시에 1회 실행 (대부분 실수)
```

`forEach`의 인자로 들어가는 함수는 **매번 실행**되는 콜백입니다.

```js
[1, 2, 3].forEach((n) => console.log(n));  // 1, 2, 3 순서대로
```

## 5. 객체 단축 표기법과 Computed keys

```js
const name = "Kim";
const age = 20;
const user = { name, age };        // { name: "Kim", age: 20 }

// Computed key
const key = "score";
const obj = { [key]: 100 };        // { score: 100 }
```

## 6. `Object.keys` / `values` / `entries`

```js
const scores = { kim: 90, lee: 85 };

Object.keys(scores);     // ['kim', 'lee']
Object.values(scores);   // [90, 85]
Object.entries(scores);  // [['kim', 90], ['lee', 85]]

Object.entries(scores).forEach(([name, score]) => {
  console.log(`${name}: ${score}`);
});
```

## 7. `async/await` 심화 — `Promise.all`

여러 비동기 작업을 **동시에** 기다릴 때:

```js
const [user, config] = await Promise.all([
  fetchUser(),
  fetchConfig(),
]);
```

순차적으로 기다릴 때보다 **훨씬 빠릅니다**.

## 8. 모듈 심화 — `import` 다양한 형태

```js
// 기본 + 이름 동시
import multiply, { add, PI } from './math.js';

// 이름 바꾸기
import { add as plus } from './math.js';

// 전체를 객체로
import * as math from './math.js';
math.add(1, 2);
```

## 9. TypeScript — `any` / `unknown` / `never`

```ts
let a: any = 1;
a.foo.bar;            // 검사 없이 통과 (위험)

// unknown: 쓰기 전에 타입 좁히기 필수
let u: unknown = JSON.parse(text);
if (typeof u === "object" && u !== null) {
  // 여기서부터 안전하게 사용 가능
}

// never: 절대 정상 종료하지 않는 함수
function throwError(msg: string): never {
  throw new Error(msg);
}
```

> React의 `catch (e)`에서 `e`의 타입이 `unknown`입니다. 25강의 `e instanceof Error ? e.message : "..."` 패턴이 바로 이 안전장치입니다.

## 10. `interface extends`와 `type`

```ts
interface User {
  id: number;
  name: string;
}

interface Admin extends User {  // User 상속
  role: string;
}

// type alias도 가능 (객체·유니언·함수 다 OK)
type ID = number | string;
type Calculator = (a: number, b: number) => number;
```

## 11. 옵셔널 `?` + `readonly`

```ts
interface Props {
  title: string;
  subtitle?: string;     // 있어도 되고 없어도 됨
  readonly id: number;   // 읽기 전용
}
```

> Day 1의 `interface HelloProps { name: string; age?: number }` 가 옵셔널 사용 예시입니다.

## 12. 타입 좁히기 (Narrowing)

`if`문 안에서 타입이 자동으로 좁아집니다.

```ts
function format(value: string | number) {
  if (typeof value === "string") {
    return value.toUpperCase();  // string
  }
  return value.toFixed(2);       // number
}

function getName(user: User | null) {
  if (!user) return "이름 없음";
  return user.name;  // user는 User로 좁혀짐
}
```

## 13. 타입 단언 (`as`, `!`)

타입스크립트가 추론하기 어려운 경우 **강제로** 타입을 지정합니다.

```ts
// getElementById는 HTMLElement | null 반환
const input = document.getElementById("email") as HTMLInputElement;
input.value = "test@mail.com";

const data = JSON.parse(text) as User;  // 파싱 결과를 User로 단언

// non-null 단언 (!)
const el = document.querySelector("input")!;  // null이 아님을 보장
el.focus();
```

> `!`는 정말 확실할 때만 쓰세요. 잘못 쓰면 런타임 에러가 납니다.

## 14. 제네릭 `<T>`

타입을 **변수처럼** 받아 재사용 가능한 컴포넌트/함수를 만듭니다.

```ts
function first<T>(arr: T[]): T {
  return arr[0];
}

const n = first<number>([1, 2, 3]);  // n: number
const s = first(["a", "b"]);          // 추론: string
```

Day 1의 `useState<Todo[]>([])` 가 제네릭 사용 예시입니다.

## 예제 코드

```txt
26-React-사전학습-정리/examples/pre-learning.html
```

## 실행 방법

1. `examples` 폴더를 엽니다.
2. `pre-learning.html`을 더블클릭합니다.
3. 각 섹션의 버튼을 눌러 콘솔(F12)에서 출력을 확인합니다.

## 따라 해보기

`Object.entries({ kim: 90, lee: 85 })`를 `for...of` 대신 `forEach`로 순회해 콘솔에 `이름: 점수` 형태로 출력해 보세요.

## 자주 하는 실수

- 옵셔널 체이닝과 일반 `&&` 단축평가를 헷갈림 (`a?.b` vs `a && a.b`)
- `const` 배열/객체는 내부 변경이 **된다**는 걸 잊음
- `addEventListener`에 함수를 등록하면서 괄호를 붙여 즉시 실행시킴
- `any`를 남발해 타입스크립트의 이점을 없앰
- `!`를 남발해 런타임에 `Cannot read property of null` 에러 발생

## 연습문제

1. `{ a: { b: { c: 1 } } }`에서 `a.b.c`가 없을 수 있다고 가정하고 옵셔널 체이닝으로 안전하게 접근하세요.
2. `unknown` 타입 변수를 객체로 좁혀서 `.name` 속성을 안전하게 읽는 코드를 작성하세요.
3. `interface Animal`을 만들고, `interface Dog extends Animal`에서 `breed: string`을 추가하세요.

## 정답 예시

```ts
// 1. 옵셔널 체이닝
const data: { a?: { b?: { c?: number } } } = {};
console.log(data.a?.b?.c); // undefined

// 2. unknown 좁히기
const u: unknown = JSON.parse('{"name":"Kim"}');
if (typeof u === "object" && u !== null && "name" in u) {
  console.log((u as { name: string }).name); // 'Kim'
}

// 3. interface extends
interface Animal { name: string; }
interface Dog extends Animal { breed: string; }
const d: Dog = { name: "Coco", breed: "Poodle" };
```

## 요약

- `const`는 재할당만 막고, 내부 값 변경은 가능 → React에선 **항상 새 객체**로 교체
- 옵셔널 체이닝 `?.`로 안전하게 속성에 접근
- Truthy/Falsy를 이해해 조건식을 간결하게
- 콜백 등록 시 **괄호 없이** 함수만 전달
- TypeScript: `any` < `unknown`(안전) < `never`(끝나지 않음)
- `interface extends`, `type`, `?`, `readonly`, 타입 좁히기, `as`/`!`, 제네릭 `<T>`는 React 코드에서 매일 만남

## 다음 강의

이제 [frontend_lecture/day1](../frontend_lecture/day1/README.md)로 이동해 React + TypeScript 5일 과정을 시작하세요. 부족한 부분은 22~25강 + 이 26강으로 돌아와 보충하면 됩니다.