# 12강. DOM 기초

## 학습 목표

- DOM이 무엇인지 이해합니다.
- JavaScript로 HTML 요소를 선택합니다.
- 선택한 요소의 글자와 스타일을 바꿉니다.

## 쉬운 설명

DOM은 브라우저가 HTML을 JavaScript로 다룰 수 있게 만든 구조입니다. 쉽게 말해, HTML 문서를 JavaScript가 만질 수 있는 객체로 바꾼 것입니다.

예를 들어 HTML에 이런 요소가 있을 때:

```html
<h1 id="title">안녕하세요</h1>
```

JavaScript로 선택할 수 있습니다.

```js
const title = document.querySelector("#title");
title.textContent = "JavaScript로 바꾼 제목";
```

## 자주 쓰는 선택 방법

```js
document.querySelector("#id이름");
document.querySelector(".class이름");
document.querySelector("태그이름");
```

## 예제 코드

예제 파일:

```txt
12-DOM기초/examples/dom-basic.html
```

## 실행 방법

1. `dom-basic.html`을 엽니다.
2. 화면의 제목과 문장이 바뀌는지 확인합니다.
3. JavaScript 코드에서 바뀌는 문장을 수정해 봅니다.

## 따라 해보기

```js
const box = document.querySelector("#box");
box.style.backgroundColor = "yellow";
box.textContent = "내용이 바뀌었습니다.";
```

## 자주 하는 실수

- HTML 요소보다 JavaScript가 먼저 실행되어 요소를 못 찾음
- `#`과 `.`을 헷갈림
- `textContent`를 `textcontent`처럼 대소문자를 틀림

## 연습문제

1. `h1` 제목을 JavaScript로 바꿔 보세요.
2. 문단의 글자색을 파란색으로 바꿔 보세요.
3. 빈 `div`에 글자를 넣어 보세요.

## 정답 예시

```js
const title = document.querySelector("h1");
title.textContent = "DOM 연습 중";
title.style.color = "blue";
```

## 요약

- DOM은 JavaScript가 HTML을 조작할 수 있게 해 줍니다.
- `querySelector()`로 요소를 선택합니다.
- `textContent`, `style`로 내용을 바꿀 수 있습니다.

## 다음 강의

[13강. 이벤트](../13-이벤트)
