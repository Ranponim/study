# 22강. ES6+ 핵심 문법

## 학습 목표

- 배열과 객체를 한 줄로 풀어 쓰는 **구조분해**를 이해합니다.
- 객체/배열을 복사·합치는 **spread / rest**를 익힙니다.
- 문자열을 깔끔히 합치는 **템플릿 리터럴**을 배웁니다.
- 값을 간단히 고르는 **삼항 연산자**와 **화살표 함수**의 다양한 형태를 사용합니다.

## 쉬운 설명

지금까지 배운 `let`, `const`, `if`, `for`, 함수, 배열, 객체는 JavaScript의 뼈대입니다. 여기에 ES6(2015년 이후) 이후에 추가된 문법 몇 개만 더 알면 React 코드가 한결 읽기 쉬워집니다. 이 강의에서는 React 코드에 **바로** 등장하는 6가지 패턴만 골라서 정리합니다.

## 1. 구조분해 할당 (Destructuring)

배열이나 객체의 값을 **한 번에 여러 변수**에 꺼내는 문법입니다.

### 배열

```js
const fruits = ["사과", "바나나", "포도"];
const [first, second] = fruits;
console.log(first);  // 사과
console.log(second); // 바나나

// 일부만 건너뛰기
const [, , third] = fruits;
console.log(third); // 포도
```

> 💡 React의 `const [count, setCount] = useState(0)`가 바로 이 배열 구조분해입니다.

### 객체

```js
const user = { name: "민수", age: 20, city: "Seoul" };
const { name, age } = user;
console.log(name); // 민수
console.log(age);  // 20

// 다른 이름으로 받기
const { name: userName } = user;
console.log(userName); // 민수
```

### 함수 매개변수

```js
// React의 Props가 이 모양으로 들어옵니다.
function greet({ name, age }) {
  console.log(`${name}님, ${age}살이시네요.`);
}

greet({ name: "영희", age: 25 });
```

## 2. 기본값 (Default values)

값이 없거나 `undefined`일 때 대신 쓸 값을 미리 정해 둡니다.

```js
// 매개변수 기본값
function greet(name = "손님") {
  console.log(`안녕하세요, ${name}님!`);
}

greet();        // 안녕하세요, 손님님!
greet("민수");  // 안녕하세요, 민수님!

// 구조분해 + 기본값
function printUser({ name = "익명", age = 0 } = {}) {
  console.log(`${name} (${age}살)`);
}

printUser({ name: "영웅" }); // 영웅 (0살)
printUser();                 // 익명 (0살)
```

> 💡 `function Counter({ start = 0, step = 1 })` 처럼 React Props의 기본값을 정할 때 꼭 쓰입니다.

## 3. Spread / Rest 연산자 (`...`)

점 세 개 `...` 하나로 **펼치거나 / 나머지를 모으는** 만능 문법입니다.

### 배열 spread — 복사·합치기

```js
const a = [1, 2];
const b = [3, 4];
const merged = [...a, ...b];
console.log(merged); // [1, 2, 3, 4]

const withExtra = [...a, 99];
console.log(withExtra); // [1, 2, 99]
```

### 객체 spread — 복사·덮어쓰기 (React 상태 업데이트의 핵심!)

```js
const user = { name: "민수", age: 20 };
const updated = { ...user, age: 21 }; // age만 덮어쓰기
console.log(updated); // { name: "민수", age: 21 }
```

> 💡 React에서 상태를 바꿀 때는 **반드시 새 객체를 만들어** 전달해야 합니다. `{ ...t, done: !t.done }` 처럼요.

### Rest — 나머지 모으기

```js
const [first, ...rest] = [1, 2, 3, 4];
console.log(first); // 1
console.log(rest);  // [2, 3, 4]

function sum(...nums) {
  return nums.reduce((a, b) => a + b, 0);
}
console.log(sum(1, 2, 3, 4)); // 10
```

## 4. 템플릿 리터럴 (백틱 문자열)

따옴표 대신 **백틱(`` ` ``)**으로 감싸면 `${}`로 변수를 끼워 넣을 수 있습니다.

```js
const name = "민수";
const age = 20;

// 옛날 방식
console.log(name + "님은 " + age + "살입니다.");

// 템플릿 리터럴
console.log(`${name}님은 ${age}살입니다.`);
```

여러 줄 문자열도 쉽게 만들 수 있습니다.

```js
const html = `
  <div>
    <h1>${name}</h1>
  </div>
`;
```

> 💡 React에서 `className={\`btn ${isActive ? "on" : ""}\`}` 처럼 동적 클래스명을 만들 때 반드시 필요합니다.

## 5. 삼항 연산자

`조건 ? 참일 때 값 : 거짓일 때 값` 형태로 **값**을 골라 반환합니다.

```js
const age = 20;
const status = age >= 19 ? "성인" : "미성년자";
console.log(status); // 성인

// React에서 조건부 렌더링
const message = isLogin ? "환영합니다!" : "로그인이 필요합니다.";
```

> `if/else`는 **문**(statement)이고, 삼항 연산자는 **식**(expression)이라서 변수에 담을 수 있습니다.

## 6. 화살표 함수 심화

07강에서 `const add = (a, b) => { return a + b; }`를 배웠습니다. 여기서 더 줄일 수 있습니다.

### 한 줄이면 `return` 생략 (암묵적 반환)

```js
const double = (x) => x * 2;
console.log(double(5)); // 10
```

### 인자가 없으면 `()`

```js
const greet = () => "안녕하세요!";
console.log(greet()); // 안녕하세요!
```

### 객체를 반환할 때는 괄호로 감싸기 ⚠️

```js
// ❌ 화살표 다음 {를 함수 본문으로 착각
const make1 = (n) => { name: n };

// ✅ 객체를 값으로 반환하려면 ( )로 감싸기
const make2 = (n) => ({ name: n });
console.log(make2("영웅")); // { name: "영웅" }
```

> 💡 Zustand의 `set((s) => ({ count: s.count + 1 }))` 가 바로 이 패턴입니다. `(s) => ({...})` — 괄호가 없으면 에러!

## 예제 코드

```txt
22-ES6-핵심-문법/examples/modern-syntax.html
```

## 실행 방법

1. `examples` 폴더를 엽니다.
2. `modern-syntax.html`을 더블클릭합니다.
3. `F12` → `Console` 탭에서 출력 결과를 확인합니다.

## 따라 해보기

배열 spread를 이용해 `[1, 2, 3]`을 복사하고 끝에 `0`을 추가한 새 배열을 만들어 보세요.

```js
const nums = [1, 2, 3];
const newNums = [...nums, 0];
console.log(newNums);
```

## 자주 하는 실수

- 구조분해에서 `=` 기본값을 안 적어 객체가 `undefined`일 때 에러가 남
- 화살표 함수에서 객체를 반환할 때 괄호 `()`를 빼먹음
- spread와 rest를 헷갈려서 같은 자리에 섞어 씀

## 연습문제

1. 배열 `[10, 20, 30]`을 구조분해로 `a`, `b`, `c`에 담으세요.
2. 객체 `{ title: "공부", done: false }`를 spread로 복사하고 `done`만 `true`로 바꾼 새 객체를 만드세요.
3. 삼항 연산자로 점수가 60 이상이면 `"합격"`, 아니면 `"불합격"`을 반환하는 표현식을 작성하세요.

## 정답 예시

```js
const [a, b, c] = [10, 20, 30];
console.log(a, b, c);

const todo = { title: "공부", done: false };
const updated = { ...todo, done: true };
console.log(updated);

const score = 75;
const result = score >= 60 ? "합격" : "불합격";
console.log(result); // 합격
```

## 요약

- 구조분해는 배열/객체의 값을 한 번에 여러 변수에 꺼냅니다.
- `...` 는 spread(펼치기) 또는 rest(나머지 모으기)로 쓰입니다.
- 템플릿 리터럴(백틱)은 `${}`로 값을 끼워 넣습니다.
- 삼항 연산자는 조건에 따라 **값**을 골라 반환합니다.
- 화살표 함수가 객체를 반환할 때는 `( )`로 감싸야 합니다.

## 다음 강의

[23강. 배열과 문자열 심화](../23-배열과-문자열-심화)