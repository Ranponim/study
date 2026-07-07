# 17강. 비동기, Promise, fetch

## 학습 목표

- 비동기가 필요한 이유를 이해합니다.
- Promise와 `async/await`의 기본 형태를 봅니다.
- `fetch()`로 외부 데이터를 가져오는 감각을 익힙니다.

## 쉬운 설명

비동기는 시간이 걸리는 일을 기다리는 동안 브라우저가 멈추지 않게 하는 방식입니다. 예를 들어 서버에서 데이터를 받아오는 일은 시간이 걸릴 수 있습니다.

## Promise 맛보기

```js
const promise = new Promise(function (resolve) {
  setTimeout(function () {
    resolve("완료!");
  }, 1000);
});

promise.then(function (message) {
  console.log(message);
});
```

## async/await

```js
async function loadData() {
  const result = await promise;
  console.log(result);
}

loadData();
```

## fetch

```js
async function getPost() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
  const data = await response.json();
  console.log(data);
}

getPost();
```

네트워크가 안 되면 실패할 수 있으므로 실제 코드에서는 `try/catch`를 사용합니다.

## 예제 코드

예제 파일:

```txt
17-비동기-promise-fetch/examples/fetch.html
```

## 실행 방법

1. 예제 파일을 엽니다.
2. 버튼을 눌러 데이터를 불러옵니다.
3. 인터넷 연결이 꺼져 있을 때 실패 메시지를 확인해 보세요.

## 자주 하는 실수

- `await`를 `async` 함수 밖에서 사용함
- `response.json()`에도 `await`가 필요하다는 점을 잊음
- 네트워크 실패를 고려하지 않음

## 연습문제

1. 버튼을 누르면 1초 뒤 “완료”를 표시하세요.
2. `fetch()` 결과의 제목만 화면에 표시하세요.
3. 실패하면 “데이터를 불러오지 못했습니다”를 표시하세요.

## 정답 예시

```js
async function loadPost() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
    const post = await response.json();
    console.log(post.title);
  } catch (error) {
    console.log("실패했습니다", error);
  }
}
```

## 요약

- 비동기는 시간이 걸리는 작업을 다루는 방식입니다.
- `async/await`를 쓰면 비동기 코드를 읽기 쉽게 쓸 수 있습니다.
- `fetch()`는 서버에서 데이터를 가져올 때 사용합니다.

## 다음 강의

[18강. 모듈](../18-모듈)
