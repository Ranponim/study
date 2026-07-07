# 07강. 함수

## 학습 목표

- 함수를 만드는 이유를 이해합니다.
- 매개변수와 반환값을 사용합니다.
- 같은 코드를 재사용하는 방법을 배웁니다.

## 쉬운 설명

함수는 자주 사용하는 코드를 이름 붙여 저장해 둔 것입니다. 필요할 때 함수 이름을 부르면 안의 코드가 실행됩니다.

```js
function sayHello() {
  console.log("안녕하세요!");
}

sayHello();
```

## 매개변수

매개변수는 함수에 전달하는 값입니다.

```js
function greet(name) {
  console.log(name + "님, 안녕하세요!");
}

greet("민수");
greet("영희");
```

## 반환값

함수는 계산 결과를 돌려줄 수 있습니다.

```js
function add(a, b) {
  return a + b;
}

const result = add(3, 5);
console.log(result);
```

## 화살표 함수 맛보기

```js
const multiply = (a, b) => {
  return a * b;
};
```

처음에는 `function` 문법을 먼저 익히면 됩니다.

## 예제 코드

예제 파일:

```txt
07-함수/examples/functions.html
```

## 실행 방법

1. `functions.html`을 엽니다.
2. 버튼과 콘솔 결과를 확인합니다.
3. 함수 이름과 매개변수를 바꿔 봅니다.

## 자주 하는 실수

- 함수를 만들기만 하고 호출하지 않음
- `return`과 `console.log()`를 같은 것으로 생각함
- 매개변수 이름과 실제 전달값을 헷갈림

## 연습문제

1. 이름을 받아 인사말을 반환하는 함수를 만드세요.
2. 두 숫자를 받아 곱한 결과를 반환하는 함수를 만드세요.
3. 나이를 받아 성인 여부를 반환하는 함수를 만드세요.

## 정답 예시

```js
function isAdult(age) {
  return age >= 19;
}

console.log(isAdult(20));
console.log(isAdult(15));
```

## 요약

- 함수는 코드를 재사용하기 위한 도구입니다.
- 매개변수는 함수에 들어가는 값입니다.
- `return`은 함수 밖으로 결과를 돌려줍니다.

## 다음 강의

[08강. 배열](../08-배열)
