# 11강. 스코프와 실행 흐름

## 학습 목표

- 스코프가 무엇인지 이해합니다.
- 변수가 사용할 수 있는 범위를 구분합니다.
- 실행 흐름과 초보자가 자주 만나는 함정을 배웁니다.

## 쉬운 설명

스코프는 변수를 사용할 수 있는 범위입니다. 집 안에 있는 물건은 집 안에서 쓸 수 있지만, 다른 집에서는 바로 쓸 수 없는 것과 비슷합니다.

## 전역 스코프

함수 밖에서 만든 변수는 여러 곳에서 사용할 수 있습니다.

```js
const message = "안녕하세요";

function sayMessage() {
  console.log(message);
}

sayMessage();
```

## 함수 스코프

함수 안에서 만든 변수는 함수 밖에서 사용할 수 없습니다.

```js
function makeName() {
  const name = "민수";
  console.log(name);
}

makeName();
// console.log(name); // 오류
```

## 블록 스코프

`if`, `for`의 중괄호 안에서 만든 `let`, `const`도 그 안에서만 사용할 수 있습니다.

```js
if (true) {
  const result = "성공";
  console.log(result);
}
```

## 실행 흐름

JavaScript는 기본적으로 위에서 아래로 실행됩니다. 함수는 정의만으로 실행되지 않고, 호출해야 실행됩니다.

## 예제 코드

예제 파일:

```txt
11-스코프와실행흐름/examples/scope.html
```

## 실행 방법

1. `scope.html`을 엽니다.
2. 콘솔에 출력되는 순서를 확인합니다.
3. 주석 처리된 오류 코드를 하나씩 풀어 보며 에러를 읽어 봅니다.

## 자주 하는 실수

- 함수 안에서 만든 변수를 함수 밖에서 사용하려 함
- 코드가 작성된 순서와 실행 순서를 헷갈림
- 오류 메시지를 읽지 않고 바로 포기함

## 연습문제

1. 함수 안에서 `message` 변수를 만들고 출력하세요.
2. 함수 밖에서 그 변수를 출력하면 어떻게 되는지 확인하세요.
3. `if` 블록 안에서 만든 변수를 밖에서 사용해 보세요.

## 정답 예시

```js
function testScope() {
  const message = "함수 안 변수";
  console.log(message);
}

testScope();
// console.log(message); // ReferenceError
```

## 요약

- 스코프는 변수를 사용할 수 있는 범위입니다.
- `let`과 `const`는 블록 스코프를 가집니다.
- 함수는 호출해야 실행됩니다.

## 다음 강의

[12강. DOM 기초](../12-DOM기초)
