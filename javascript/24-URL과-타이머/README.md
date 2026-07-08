# 24강. URL 다루기와 타이머

## 학습 목표

- URL에 한글·특수문자를 안전하게 넣는 `encodeURIComponent`를 사용합니다.
- 쿼리스트링을 다루는 `URLSearchParams`의 기본을 익힙니다.
- `setTimeout` / `setInterval`으로 **지연 실행**과 **반복 실행**을 합니다.
- 타이머를 **취소**하는 패턴을 배웁니다.

## 쉬운 설명

페이지를 새로 고치지 않고 URL만 바꾸며 데이터를 실어 나르는 게 React Router의 핵심 동작입니다. 또 입력할 때마다 검색하는 게 아니라 “타이핑이 멈춘 뒤” 한 번만 검색하려면 타이머를 알아야 합니다. 이 강의에서는 URL 인코딩과 브라우저 타이머 두 가지를 함께 다룹니다.

## 1. `encodeURIComponent` / `decodeURIComponent`

URL에는 ASCII 문자(영문, 숫자, 일부 기호)만 안전하게 들어갈 수 있습니다. 한글, 공백, `&`, `=` 같은 글자는 그대로 넣으면 깨지거나 의미가 바뀌기 때문에 **퍼센트 인코딩**으로 바꿔야 합니다.

```js
const keyword = "어벤져스 엔드게임";

const encoded = encodeURIComponent(keyword);
console.log(encoded);
// %EC%96%B4%EB%B2%A0%EC%A0%B8%EC%8A%A4%20%EC%97%94%EB%93%9C%EA%B2%8C%EC%9E%84

const decoded = decodeURIComponent(encoded);
console.log(decoded); // 어벤져스 엔드게임
```

> 💡 React에서 검색 페이지를 만들 때 꼭 필요합니다.
> ```js
> navigate(`/movies?q=${encodeURIComponent(keyword)}`)
> ```
> 안 넣으면 `"어벤져스"`의 공백 때문에 URL이 깨집니다.

## 2. `URLSearchParams` API

`?q=avengers&page=2` 같은 쿼리스트링을 쉽게 다룰 수 있는 표준 도구입니다.

```js
// 쿼리스트링 → 객체
const params = new URLSearchParams("q=avengers&page=2");
console.log(params.get("q"));    // "avengers"
console.log(params.get("page")); // "2"
console.log(params.has("page")); // true

// 객체 → 쿼리스트링
const next = new URLSearchParams({ q: "interstellar", page: "1" });
console.log(next.toString());
// q=interstellar&page=1
```

> 💡 React Router의 `useSearchParams`도 내부적으로 이 객체와 비슷한 형태로 동작합니다.

## 3. `window.location` 읽기

현재 페이지의 URL 정보를 읽을 수 있습니다 (강의 수준에서는 읽기만).

```js
console.log(window.location.href);   // 현재 페이지 전체 URL
console.log(window.location.search); // 쿼리스트링 부분만 (?q=avengers)
console.log(window.location.pathname); // 경로 부분 (/movies)
```

## 4. `setTimeout` / `clearTimeout` — 1회 지연 실행

일정 시간 뒤에 **한 번만** 실행하고 싶을 때 사용합니다.

```js
// 1초 뒤 한 번 실행
setTimeout(() => {
  console.log("1초 지남!");
}, 1000);

// 취소도 가능
const timer = setTimeout(() => {
  console.log("이건 실행 안 됨");
}, 1000);
clearTimeout(timer);
```

> 💡 React의 `useEffect`에서 자주 쓰는 디바운싱 패턴입니다.
> ```js
> useEffect(() => {
>   const timer = setTimeout(() => navigate(...), 500);
>   return () => clearTimeout(timer); // 다음 effect 전에 취소
> }, [keyword]);
> ```

## 5. `setInterval` / `clearInterval` — 반복 실행

일정 간격으로 **계속** 실행하고 싶을 때 사용합니다.

```js
let count = 0;
const id = setInterval(() => {
  count += 1;
  console.log(`${count}초 경과`);
  if (count >= 5) {
    clearInterval(id); // 5초 뒤 멈춤
  }
}, 1000);
```

## 6. Cleanup 패턴 — 누수 방지

`setInterval`을 `clearInterval`로 멈추지 않으면 페이지가 닫혀도 백그라운드에서 계속 돌아갑니다. **항상 ID를 저장해 두었다가 필요 없어지면 정리**하세요.

```js
let timerId = null;

function start() {
  if (timerId !== null) return;
  timerId = setInterval(() => {
    console.log("tick");
  }, 1000);
}

function stop() {
  if (timerId !== null) {
    clearInterval(timerId);
    timerId = null;
  }
}

start();
// ... 잠시 후
stop();
```

## 예제 코드

```txt
24-URL과-타이머/examples/url-timer.html
```

## 실행 방법

1. `examples` 폴더를 엽니다.
2. `url-timer.html`을 더블클릭합니다.
3. 입력창에 검색어를 넣고 “URL 만들기” 버튼을 누릅니다.
4. “시작 / 정지” 버튼으로 타이머를 테스트합니다.
5. `F12` → `Console`에서 출력도 함께 봅니다.

## 따라 해보기

`setTimeout`을 이용해 버튼을 누른 **3초 뒤**에 콘솔에 “3초 지났습니다!”를 출력해 보세요.

```js
const button = document.querySelector("#myButton");
button.addEventListener("click", () => {
  setTimeout(() => {
    console.log("3초 지났습니다!");
  }, 3000);
});
```

## 자주 하는 실수

- URL에 한글/공백을 그대로 넣어 페이지가 깨짐 → `encodeURIComponent` 필수
- `setInterval`을 시작만 하고 `clearInterval`을 안 해서 메모리/성능 문제 발생
- `setTimeout`의 지연 시간을 **ms 단위**로 적어야 한다는 걸 잊고 `1` 같은 초 단위를 씀

## 연습문제

1. `"Hello World!"`를 `encodeURIComponent`로 인코딩한 결과를 출력하세요.
2. `new URLSearchParams("name=민수&age=20")`에서 `name`과 `age` 값을 각각 출력하세요.
3. 1초마다 “tick”을 출력하고, 5번 출력되면 멈추는 `setInterval` 코드를 작성하세요.

## 정답 예시

```js
console.log(encodeURIComponent("Hello World!"));
// Hello%20World%21

const params = new URLSearchParams("name=민수&age=20");
console.log(params.get("name")); // 민수
console.log(params.get("age"));  // 20

let count = 0;
const id = setInterval(() => {
  console.log("tick");
  count += 1;
  if (count >= 5) clearInterval(id);
}, 1000);
```

## 요약

- URL에 한글·공백을 넣을 때는 `encodeURIComponent`로 인코딩합니다.
- `URLSearchParams`로 쿼리스트링을 객체처럼 다룰 수 있습니다.
- `setTimeout`은 1회, `setInterval`은 반복 실행입니다.
- 반드시 `clearTimeout` / `clearInterval`로 정리해 누수를 막습니다.

## 다음 강의

[25강. HTTP와 에러 처리](../25-HTTP와-에러-처리)