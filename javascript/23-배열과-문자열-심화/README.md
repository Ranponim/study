# 23강. 배열과 문자열 심화

## 학습 목표

- `find()`로 조건에 맞는 **하나**를 찾습니다.
- `includes()`, `some()`, `every()`로 포함 여부를 검사합니다.
- 문자열을 다듬는 `trim()`과 자르는 `split()`/`join()`을 사용합니다.

## 쉬운 설명

10강에서 `forEach`, `map`, `filter`를 배웠습니다. 이 3개만으로도 많은 일을 할 수 있지만, React 코드를 읽다 보면 `find`, `includes`, `trim` 같은 메서드가 **자주** 등장합니다. 이 강의에서는 “검색·확인·다듬기”에 자주 쓰는 메서드만 추가로 익힁니다.

## 1. `Array.find()` — 조건에 맞는 첫 번째 값 찾기

`filter`는 **배열**을 돌려주고, `find`는 **하나의 값**만 돌려줍니다.

```js
const users = [
  { id: 1, name: "민수" },
  { id: 2, name: "영희" },
  { id: 3, name: "지수" },
];

const user = users.find((u) => u.id === 2);
console.log(user); // { id: 2, name: "영희" }

// 없으면 undefined
const missing = users.find((u) => u.id === 99);
console.log(missing); // undefined
```

> 💡 React에서 “특정 id의 할 일을 찾아서 수정”할 때 거의 항상 이 패턴입니다.
> ```js
> const target = todos.find((t) => t.id === id);
> ```

## 2. `Array.includes()` — 포함 여부 확인

```js
const fruits = ["사과", "바나나", "포도"];

console.log(fruits.includes("사과")); // true
console.log(fruits.includes("딸기")); // false

// 문자열도 가능
console.log("hello world".includes("world")); // true
```

## 3. `Array.some()` / `Array.every()` — 조건 만족 여부

```js
const numbers = [1, 5, 8, 12];

console.log(numbers.some((n) => n > 10));  // true (12가 10보다 큼)
console.log(numbers.every((n) => n > 0));  // true (모두 양수)

console.log(numbers.some((n) => n < 0));   // false
console.log(numbers.every((n) => n > 5));  // false
```

- `some` — “하나라도?” 라는 뜻 (OR)
- `every` — “전부?” 라는 뜻 (AND)

## 4. `Array.indexOf()` — 위치 찾기

```js
const fruits = ["사과", "바나나", "포도"];
console.log(fruits.indexOf("바나나")); // 1
console.log(fruits.indexOf("딸기"));   // -1 (없음)
```

## 5. `String.trim()` — 앞뒤 공백 제거

사용자가 입력한 값에는 의도하지 않은 공백이 섞여 있을 수 있습니다. `trim()`으로 깨끗이 다듬습니다.

```js
const input = "   안녕하세요   ";
console.log(input.trim()); // "안녕하세요"

const name = "  민수 ";
if (name.trim() === "") {
  console.log("이름을 입력해 주세요.");
}
```

> 💡 React에서 입력값으로 새 항목을 추가하기 전 `get().input.trim()` 처럼 빈 문자열 검사에 자주 쓰입니다.

## 6. `String.includes()` — 부분 문자열 확인

```js
const url = "https://example.com";
console.log(url.includes("https")); // true
console.log(url.includes("http"));  // true (https도 http을 포함)
```

## 7. `String.split()` / `Array.join()` — 문자열 ↔ 배열

```js
// split: 문자열 → 배열
const tags = "javascript,react,vue";
const list = tags.split(",");
console.log(list); // ["javascript", "react", "vue"]

// join: 배열 → 문자열
const words = ["2024", "01", "15"];
const date = words.join("-");
console.log(date); // "2024-01-15"
```

## 8. `String.startsWith()` / `endsWith()` — 시작/끝 확인

```js
const file = "photo.png";
console.log(file.startsWith("photo")); // true
console.log(file.endsWith(".png"));    // true
```

## 예제 코드

```txt
23-배열과-문자열-심화/examples/array-string-advanced.html
```

## 실행 방법

1. `examples` 폴더를 엽니다.
2. `array-string-advanced.html`을 더블클릭합니다.
3. 입력창에 글자를 넣고 버튼을 눌러 결과를 확인합니다.
4. `F12` → `Console`에서 출력도 함께 봅니다.

## 따라 해보기

`users` 배열에서 `age`가 20 이상인 **첫 번째** 사용자를 찾아 콘솔에 출력해 보세요.

```js
const users = [
  { name: "민수", age: 15 },
  { name: "영희", age: 22 },
  { name: "지수", age: 30 },
];

const adult = users.find((u) => u.age >= 20);
console.log(adult);
```

## 자주 하는 실수

- `find` 대신 `filter`를 써서 결과가 배열인지 객체인지 헷갈림
- `includes`와 `indexOf`의 차이를 모르고 섞어 씀 (`includes`는 true/false, `indexOf`는 숫자/-1)
- `trim()`이 원래 문자열을 바꾸는 줄 알고 `input.trim()`의 결과를 다시 변수에 안 담음

## 연습문제

1. 상품 배열에서 가격이 10,000원 이상인 첫 번째 상품의 이름을 출력하세요.
2. `"  hello world  "`의 앞뒤 공백을 제거하고, 단어를 배열로 나누세요.
3. 숫자 배열 `[3, 7, 12, 1]`에 10보다 큰 수가 하나라도 있는지 `some`으로 확인하세요.

## 정답 예시

```js
const products = [
  { name: "연필", price: 500 },
  { name: "노트", price: 3000 },
  { name: "책상", price: 120000 },
];

const expensive = products.find((p) => p.price >= 10000);
console.log(expensive.name); // 책상

const greeting = "  hello world  ".trim();
const words = greeting.split(" ");
console.log(words); // ["hello", "world"]

const nums = [3, 7, 12, 1];
const hasBig = nums.some((n) => n > 10);
console.log(hasBig); // true
```

## 요약

- `find()`는 조건에 맞는 **첫 값**을, `filter()`는 **배열**을 돌려줍니다.
- `includes()`는 포함 여부(true/false), `some()`은 OR, `every()`는 AND 검사입니다.
- `trim()`은 문자열 앞뒤 공백을 제거합니다.
- `split()`은 문자열 → 배열, `join()`은 배열 → 문자열로 바꿉니다.

## 다음 강의

[24강. URL 다루기와 타이머](../24-URL과-타이머)