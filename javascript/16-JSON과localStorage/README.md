# 16강. JSON과 localStorage

## 학습 목표

- JSON이 무엇인지 이해합니다.
- 객체와 배열을 문자열로 변환합니다.
- `localStorage`에 데이터를 저장하고 불러옵니다.

## 쉬운 설명

브라우저를 새로고침하면 JavaScript 변수는 사라집니다. 데이터를 브라우저에 간단히 저장하고 싶을 때 `localStorage`를 사용할 수 있습니다.

단, `localStorage`에는 문자열만 저장할 수 있습니다. 그래서 객체나 배열은 JSON 문자열로 바꿔 저장합니다.

## JSON 변환

```js
const user = { name: "민수", age: 20 };

const text = JSON.stringify(user);
console.log(text);

const restored = JSON.parse(text);
console.log(restored.name);
```

## localStorage 저장

```js
localStorage.setItem("name", "민수");
const name = localStorage.getItem("name");
console.log(name);
```

## 객체 저장

```js
const todos = ["공부하기", "운동하기"];
localStorage.setItem("todos", JSON.stringify(todos));

const savedTodos = JSON.parse(localStorage.getItem("todos"));
console.log(savedTodos);
```

## 예제 코드

예제 파일:

```txt
16-JSON과localStorage/examples/local-storage.html
```

## 실행 방법

1. 예제 파일을 엽니다.
2. 값을 입력하고 저장합니다.
3. 페이지를 새로고침해도 값이 남아 있는지 확인합니다.

## 자주 하는 실수

- 객체를 JSON으로 바꾸지 않고 바로 저장함
- `JSON.parse()`할 값이 `null`인데 바로 사용함
- localStorage가 영구 데이터베이스라고 생각함. 브라우저 저장소이므로 중요한 보안 정보는 저장하면 안 됩니다.

## 연습문제

1. 이름을 localStorage에 저장하세요.
2. 저장한 이름을 화면에 표시하세요.
3. 좋아하는 음식 배열을 JSON으로 저장하세요.

## 정답 예시

```js
const foods = ["김밥", "라면"];
localStorage.setItem("foods", JSON.stringify(foods));

const saved = JSON.parse(localStorage.getItem("foods"));
console.log(saved);
```

## 요약

- JSON은 데이터를 문자열로 표현하는 형식입니다.
- `JSON.stringify()`는 값 → 문자열 변환입니다.
- `JSON.parse()`는 문자열 → 값 변환입니다.
- `localStorage`는 브라우저에 간단히 저장할 때 사용합니다.

## 다음 강의

[17강. 비동기, Promise, fetch](../17-비동기-promise-fetch)
