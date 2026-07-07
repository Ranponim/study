# 05강. 조건문

## 학습 목표

- 조건에 따라 다른 코드를 실행합니다.
- `if`, `else if`, `else`를 사용합니다.
- 간단한 분기 로직을 작성합니다.

## 쉬운 설명

조건문은 “만약 ~라면”을 코드로 표현하는 방법입니다.

```js
const age = 20;

if (age >= 19) {
  console.log("성인입니다.");
} else {
  console.log("미성년자입니다.");
}
```

조건이 `true`이면 첫 번째 코드가 실행되고, `false`이면 `else` 코드가 실행됩니다.

## 여러 조건

```js
const score = 85;

if (score >= 90) {
  console.log("A");
} else if (score >= 80) {
  console.log("B");
} else {
  console.log("C");
}
```

## 예제 코드

예제 파일:

```txt
05-조건문/examples/conditions.html
```

## 실행 방법

1. `conditions.html`을 엽니다.
2. 입력값 또는 코드의 점수를 바꿔 봅니다.
3. 결과 문장이 어떻게 바뀌는지 확인합니다.

## 자주 하는 실수

- 조건식 뒤에 세미콜론을 붙임

잘못된 예:

```js
if (age >= 19); {
  console.log("성인");
}
```

- 중괄호 `{}`를 빼먹음

## 연습문제

1. 온도가 30 이상이면 “덥습니다”를 출력하세요.
2. 점수에 따라 A/B/C 등급을 출력하세요.
3. 비밀번호가 `1234`이면 “로그인 성공”을 출력하세요.

## 정답 예시

```js
const password = "1234";

if (password === "1234") {
  console.log("로그인 성공");
} else {
  console.log("로그인 실패");
}
```

## 요약

- 조건문은 상황에 따라 다른 코드를 실행합니다.
- 조건식 결과는 `true` 또는 `false`입니다.
- 여러 조건은 `else if`로 연결합니다.

## 다음 강의

[06강. 반복문](../06-반복문)
