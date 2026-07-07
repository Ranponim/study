# 10강. 배열과 객체 메서드

## 학습 목표

- 배열을 순회하는 방법을 배웁니다.
- `forEach`, `map`, `filter`의 역할을 이해합니다.
- 객체 배열을 다루는 기초를 익힙니다.

## 쉬운 설명

배열 안의 값을 하나씩 꺼내 작업할 때 메서드를 사용하면 편리합니다.

## forEach

`forEach`는 배열의 값을 하나씩 꺼내 실행합니다.

```js
const fruits = ["사과", "바나나", "딸기"];

fruits.forEach(function (fruit) {
  console.log(fruit);
});
```

## map

`map`은 배열을 바탕으로 새 배열을 만듭니다.

```js
const numbers = [1, 2, 3];
const doubled = numbers.map(function (number) {
  return number * 2;
});

console.log(doubled);
```

## filter

`filter`는 조건에 맞는 값만 남깁니다.

```js
const scores = [60, 90, 40, 100];
const passed = scores.filter(function (score) {
  return score >= 70;
});

console.log(passed);
```

## 객체 배열

```js
const users = [
  { name: "민수", age: 20 },
  { name: "영희", age: 17 }
];
```

## 예제 코드

예제 파일:

```txt
10-배열과객체-메서드/examples/array-object-methods.html
```

## 실행 방법

1. 예제 파일을 엽니다.
2. 콘솔에서 각 메서드의 결과를 확인합니다.
3. 배열 값을 추가하고 결과를 다시 확인합니다.

## 자주 하는 실수

- `forEach`가 새 배열을 만든다고 착각함
- `map`에서 `return`을 빼먹음
- `filter` 조건식이 `true`/`false`를 반환해야 한다는 점을 잊음

## 연습문제

1. 숫자 배열의 모든 값을 10배로 만든 새 배열을 만드세요.
2. 점수 배열에서 80점 이상만 남기세요.
3. 사용자 배열에서 성인만 남기세요.

## 정답 예시

```js
const users = [
  { name: "민수", age: 20 },
  { name: "영희", age: 17 }
];

const adults = users.filter(function (user) {
  return user.age >= 19;
});

console.log(adults);
```

## 요약

- `forEach`는 하나씩 실행합니다.
- `map`은 새 배열을 만듭니다.
- `filter`는 조건에 맞는 값만 남깁니다.

## 다음 강의

[11강. 스코프와 실행 흐름](../11-스코프와실행흐름)
