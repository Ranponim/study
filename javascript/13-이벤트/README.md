# 13강. 이벤트

## 학습 목표

- 이벤트가 무엇인지 이해합니다.
- 클릭 이벤트를 처리합니다.
- `addEventListener()`를 사용합니다.

## 쉬운 설명

이벤트는 사용자가 웹 페이지에서 하는 행동입니다. 예를 들어 클릭, 키보드 입력, 마우스 이동 등이 이벤트입니다.

버튼을 클릭했을 때 코드를 실행하려면 다음처럼 작성합니다.

```js
const button = document.querySelector("#button");

button.addEventListener("click", function () {
  console.log("버튼을 클릭했습니다.");
});
```

## 이벤트 처리 흐름

1. HTML 요소를 선택합니다.
2. 어떤 이벤트를 기다릴지 정합니다.
3. 이벤트가 발생했을 때 실행할 함수를 작성합니다.

## 예제 코드

예제 파일:

```txt
13-이벤트/examples/events.html
```

## 실행 방법

1. `events.html`을 엽니다.
2. 버튼을 클릭합니다.
3. 화면의 숫자나 문장이 바뀌는지 확인합니다.

## 따라 해보기

```js
button.addEventListener("click", function () {
  alert("클릭했습니다!");
});
```

## 자주 하는 실수

- `addEventListener` 철자를 틀림
- 버튼을 선택하기 전에 이벤트를 붙이려고 함
- 함수 뒤에 괄호를 붙여 즉시 실행해 버림

잘못된 예:

```js
button.addEventListener("click", sayHello());
```

처음에는 익명 함수를 쓰면 안전합니다.

## 연습문제

1. 버튼을 누르면 숫자가 1씩 증가하게 하세요.
2. 버튼을 누르면 배경색이 바뀌게 하세요.
3. 버튼을 누르면 문장이 “완료!”로 바뀌게 하세요.

## 정답 예시

```js
let count = 0;
const button = document.querySelector("#button");
const result = document.querySelector("#result");

button.addEventListener("click", function () {
  count = count + 1;
  result.textContent = count;
});
```

## 요약

- 이벤트는 사용자의 행동입니다.
- `addEventListener()`로 이벤트를 기다립니다.
- 이벤트가 발생하면 함수가 실행됩니다.

## 다음 강의

[14강. 폼과 입력값](../14-폼과입력값)
