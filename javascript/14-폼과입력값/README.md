# 14강. 폼과 입력값

## 학습 목표

- 입력창의 값을 JavaScript로 읽습니다.
- 폼 제출 이벤트를 처리합니다.
- 간단한 입력 검증을 합니다.

## 쉬운 설명

웹 페이지에서 사용자가 입력한 값은 JavaScript로 읽을 수 있습니다.

```html
<input id="nameInput" />
<button id="button">확인</button>
```

```js
const input = document.querySelector("#nameInput");
const button = document.querySelector("#button");

button.addEventListener("click", function () {
  console.log(input.value);
});
```

`input.value`가 입력창의 현재 값입니다.

## form 제출 막기

폼은 기본적으로 제출되면 페이지가 새로고침될 수 있습니다. 학습 단계에서는 `event.preventDefault()`로 막고 직접 처리합니다.

```js
form.addEventListener("submit", function (event) {
  event.preventDefault();
});
```

## 예제 코드

예제 파일:

```txt
14-폼과입력값/examples/form-input.html
```

## 실행 방법

1. 예제 파일을 엽니다.
2. 이름을 입력하고 버튼을 누릅니다.
3. 화면에 인사말이 표시되는지 확인합니다.

## 따라 해보기

입력값이 비어 있으면 안내 문장을 보여 주세요.

```js
if (input.value === "") {
  result.textContent = "이름을 입력해 주세요.";
}
```

## 자주 하는 실수

- `input` 요소 자체와 `input.value`를 헷갈림
- `preventDefault()`를 빼먹어 페이지가 새로고침됨
- 입력값 앞뒤 공백을 처리하지 않음

## 연습문제

1. 이름을 입력하면 “안녕하세요, 이름님”을 표시하세요.
2. 값이 비어 있으면 “입력해 주세요”를 표시하세요.
3. 나이가 19 이상이면 “성인”을 표시하세요.

## 정답 예시

```js
const age = Number(ageInput.value);

if (age >= 19) {
  result.textContent = "성인입니다.";
} else {
  result.textContent = "미성년자입니다.";
}
```

## 요약

- 입력값은 `.value`로 읽습니다.
- 폼 제출은 `submit` 이벤트로 처리합니다.
- `preventDefault()`로 기본 새로고침을 막을 수 있습니다.

## 다음 강의

[15강. 에러와 디버깅](../15-에러와디버깅)
