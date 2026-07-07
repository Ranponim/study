# 04강. 연산자

## 학습 목표

- 산술 연산자를 사용합니다.
- 비교 연산자로 값을 비교합니다.
- 논리 연산자로 조건을 조합합니다.

## 쉬운 설명

연산자는 값을 계산하거나 비교할 때 사용하는 기호입니다.

## 산술 연산자

```js
console.log(10 + 3); // 더하기
console.log(10 - 3); // 빼기
console.log(10 * 3); // 곱하기
console.log(10 / 3); // 나누기
console.log(10 % 3); // 나머지
```

## 비교 연산자

```js
console.log(10 > 3);
console.log(10 === 10);
console.log(10 !== 5);
```

비교 결과는 항상 `true` 또는 `false`입니다.

## 논리 연산자

```js
const isAdult = true;
const hasTicket = false;

console.log(isAdult && hasTicket); // 둘 다 true여야 true
console.log(isAdult || hasTicket); // 하나라도 true면 true
console.log(!isAdult);             // 반대로 바꿈
```

## 예제 코드

예제 파일:

```txt
04-연산자/examples/operators.html
```

## 실행 방법

1. `operators.html`을 엽니다.
2. 숫자를 바꿔 봅니다.
3. 비교 결과가 어떻게 바뀌는지 확인합니다.

## 자주 하는 실수

- `=`와 `===`를 헷갈림

```js
const age = 20;      // 값을 저장
console.log(age === 20); // 같은지 비교
```

초보 단계에서는 비교할 때 `==`보다 `===`를 쓰세요.

## 연습문제

1. `price`와 `count`를 만들어 총 금액을 계산하세요.
2. 나이가 19 이상인지 비교하세요.
3. 회원이면서 쿠폰이 있는지 논리 연산자로 확인하세요.

## 정답 예시

```js
const price = 3000;
const count = 4;
console.log(price * count);

const age = 20;
console.log(age >= 19);

const isMember = true;
const hasCoupon = true;
console.log(isMember && hasCoupon);
```

## 요약

- `+`, `-`, `*`, `/`, `%`는 계산에 사용합니다.
- `===`, `!==`, `>`, `<`는 비교에 사용합니다.
- `&&`, `||`, `!`는 조건을 조합합니다.

## 다음 강의

[05강. 조건문](../05-조건문)
