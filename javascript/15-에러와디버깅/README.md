# 15강. 에러와 디버깅

## 학습 목표

- 에러 메시지를 두려워하지 않습니다.
- 브라우저 개발자 도구를 사용합니다.
- `console.log()`로 값을 확인합니다.

## 쉬운 설명

에러는 “컴퓨터가 이해하지 못한 부분을 알려주는 메시지”입니다. 에러가 나오는 것은 실패가 아니라, 고칠 위치를 알려주는 힌트입니다.

## 개발자 도구 열기

- Windows: `F12` 또는 `Ctrl + Shift + I`
- Console 탭: JavaScript 출력과 에러 확인
- Elements 탭: HTML 구조 확인

## 자주 보는 에러

### ReferenceError

없는 변수를 사용했을 때 발생합니다.

```js
console.log(userName);
```

### TypeError

값의 종류가 예상과 다를 때 발생합니다.

```js
const title = null;
title.textContent = "안녕";
```

### SyntaxError

문법이 틀렸을 때 발생합니다.

```js
console.log("안녕;
```

## 예제 코드

예제 파일:

```txt
15-에러와디버깅/examples/debugging.html
```

## 실행 방법

1. 예제 파일을 엽니다.
2. 개발자 도구 Console 탭을 엽니다.
3. 일부러 만든 로그와 안내를 확인합니다.
4. 주석 처리된 에러 코드를 하나씩 풀어 보세요.

## 디버깅 순서

1. 에러 메시지를 읽습니다.
2. 파일 이름과 줄 번호를 봅니다.
3. 해당 줄 근처의 변수 값을 `console.log()`로 확인합니다.
4. 작은 부분부터 고칩니다.

## 자주 하는 실수

- 에러 메시지를 읽지 않고 코드 전체를 바꿈
- 여러 곳을 한꺼번에 수정함
- 저장하지 않고 새로고침함

## 연습문제

1. 일부러 변수 이름을 틀려 보고 에러를 읽어 보세요.
2. 괄호 하나를 지워 보고 어떤 에러가 나는지 확인하세요.
3. `console.log()`로 입력값을 확인해 보세요.

## 정답 예시

```js
const userName = "민수";
console.log("현재 userName:", userName);
```

## 요약

- 에러 메시지는 고칠 위치를 알려 줍니다.
- Console 탭은 JavaScript 학습의 가장 중요한 도구입니다.
- 한 번에 한 가지씩 확인해야 빨리 고칠 수 있습니다.

## 다음 강의

[16강. JSON과 localStorage](../16-JSON과localStorage)
