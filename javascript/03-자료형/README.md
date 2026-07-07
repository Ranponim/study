# 03강. 자료형

## 학습 목표

- JavaScript의 기본 자료형을 이해합니다.
- 문자열, 숫자, 불리언을 구분합니다.
- `typeof`로 값의 종류를 확인합니다.

## 쉬운 설명

자료형은 값의 종류입니다. 사람에게 이름, 나이, 참/거짓 정보가 있듯이 JavaScript 값에도 종류가 있습니다.

대표적인 자료형:

```js
const name = "민수";      // 문자열 string
const age = 20;           // 숫자 number
const isStudent = true;   // 불리언 boolean
const empty = null;       // 비어 있음
let notYet;               // undefined
```

## 예제 코드

예제 파일:

```txt
03-자료형/examples/types.html
```

## 실행 방법

1. `types.html`을 엽니다.
2. 화면의 값과 콘솔의 `typeof` 결과를 확인합니다.
3. 값을 직접 바꿔 보고 결과를 다시 확인합니다.

## 따라 해보기

```js
console.log(typeof "안녕");
console.log(typeof 123);
console.log(typeof true);
```

## 자주 하는 실수

- 숫자처럼 보여도 따옴표 안에 있으면 문자열입니다.

```js
const a = 10;     // 숫자
const b = "10";   // 문자열
```

- `null`과 `undefined`를 처음부터 완벽히 구분하려고 함. 지금은 둘 다 “값이 비어 있는 상태” 정도로 이해해도 됩니다.

## 연습문제

1. 자신의 이름을 문자열로 저장하세요.
2. 좋아하는 숫자를 숫자형으로 저장하세요.
3. 오늘 공부했는지를 불리언으로 저장하세요.
4. 각각의 `typeof` 결과를 출력하세요.

## 정답 예시

```js
const myName = "지수";
const favoriteNumber = 7;
const studiedToday = true;

console.log(typeof myName);
console.log(typeof favoriteNumber);
console.log(typeof studiedToday);
```

## 요약

- 자료형은 값의 종류입니다.
- 문자열은 따옴표로 감쌉니다.
- 숫자는 따옴표 없이 씁니다.
- 불리언은 `true` 또는 `false`입니다.

## 다음 강의

[04강. 연산자](../04-연산자)
