# 25강. HTTP와 에러 처리

## 학습 목표

- HTTP 메서드(GET/POST/PUT/DELETE)의 차이를 이해합니다.
- 상태 코드(200, 404, 500)의 의미를 압니다.
- `try / catch / finally`로 실패를 안전하게 처리합니다.
- `Error` 객체와 `instanceof Error`로 에러 메시지를 읽습니다.
- `fetch`에서 `response.ok`를 확인하는 패턴을 익힙니다.

## 쉬운 설명

17강에서 `fetch`로 데이터를 가져오는 기본을 배웠습니다. 이 강의에서는 **한 단계 더** 들어가, “어떤 요청을 보내는지(메서드)”, “서버가 어떻게 답했는지(상태 코드)”, “실패하면 어떻게 처리할지(에러)”를 다룹니다. React 강의의 API 모듈을 읽으려면 이 4가지가 꼭 필요합니다.

## 1. HTTP란 무엇인가

브라우저와 서버가 **주고받는 대화의 약속**입니다.

```txt
[브라우저]  -- 요청(Request) -->  [서버]
            <-- 응답(Response) --
```

요청에는 보통 **메서드 + URL + 헤더 + 본문**이 들어가고, 응답에는 **상태 코드 + 본문**이 돌아옵니다.

## 2. HTTP 메서드 — “무엇을 하고 싶은가”

| 메서드 | 의미 | 예시 |
| --- | --- | --- |
| `GET` | 조회 (가져오기) | 영화 검색, 할 일 목록 |
| `POST` | 생성 (새로 만들기) | 새 할 일 추가, 회원가입 |
| `PUT` / `PATCH` | 수정 (바꾸기) | 할 일 완료 체크, 회원 정보 변경 |
| `DELETE` | 삭제 | 할 일 제거 |

> 💡 REST API에서는 **URL은 리소스(명사)**를, **메서드는 행위(동사)**를 나타냅니다.
> - `GET /todos` — 할 일 목록 **조회**
> - `POST /todos` — 새 할 일 **생성**
> - `PUT /todos/3` — 3번 할 일 **수정**
> - `DELETE /todos/3` — 3번 할 일 **삭제**

## 3. HTTP 상태 코드 — “결과가 어땠는가”

상태 코드는 **3자리 숫자**로, 첫 자리만 봐도 대략적 의미를 알 수 있습니다.

| 범위 | 의미 | 대표 코드 |
| --- | --- | --- |
| `2xx` | 성공 | `200` OK, `201` Created |
| `3xx` | 리다이렉트 | `301`, `302` |
| `4xx` | 클라이언트(내 코드) 잘못 | `400` 잘못된 요청, `404` 없음, `401` 인증 필요 |
| `5xx` | 서버 잘못 | `500` 서버 에러, `503` 점검 중 |

## 4. 헤더 (Headers) — “추가 정보”

요청/응답에 같이 보내는 부가 정보입니다.

```txt
Content-Type: application/json
Authorization: Bearer abc123
```

브라우저는 기본 헤더(예: `Host`)를 자동 추가하지만, **API 키나 인증 토큰**은 직접 넣어야 합니다.

## 5. `try / catch / finally` — 실패에 대비하기

17강에서 한 번 다뤘지만, 여기서 더 명확히 정리합니다.

```js
async function load() {
  try {
    const res = await fetch("https://api.example.com/data");
    const data = await res.json();
    console.log(data);
  } catch (error) {
    // 네트워크 실패, JSON 파싱 실패 등 모두 여기로 옴
    console.log("실패:", error);
  } finally {
    // 성공이든 실패든 무조건 실행
    console.log("완료");
  }
}
```

- `try` — 위험한 코드
- `catch` — 실패 시 처리
- `finally` — 성공/실패 상관없이 정리 (로딩 끄기 등)

## 6. `Error` 객체와 `throw`

`new Error("메시지")`로 직접 에러 객체를 만들 수 있고, `throw`로 의도적으로 에러를 발생시킬 수 있습니다.

```js
function divide(a, b) {
  if (b === 0) {
    throw new Error("0으로 나눌 수 없습니다.");
  }
  return a / b;
}

try {
  console.log(divide(10, 0));
} catch (e) {
  console.log(e.message); // "0으로 나눌 수 없습니다."
}
```

## 7. `instanceof Error` — 에러 타입 좁히기

`catch (e)`의 `e`는 **타입이 불분명**할 수 있습니다. 안전하게 메시지를 읽으려면 `instanceof`로 확인합니다.

```js
try {
  // ...
} catch (e) {
  const msg = e instanceof Error ? e.message : "알 수 없는 오류";
  console.log(msg);
}
```

> 💡 React의 store 액션에서 정확히 이 패턴이 등장합니다.
> ```js
> catch (e) {
>   set({ error: e instanceof Error ? e.message : "알 수 없는 오류" })
> }
> ```

## 8. `fetch`에서 `response.ok` 확인 ⚠️

`fetch`는 **404나 500을 자동으로 throw 하지 않습니다.** 직접 확인해야 합니다.

```js
async function getPost(id) {
  const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`);

  if (!res.ok) {
    throw new Error(`HTTP ${res.status}`);
  }

  return res.json();
}
```

> 💡 Axios는 4xx/5xx를 자동으로 throw 해주지만, **`fetch`는 직접 확인**해야 한다는 차이가 있습니다.

## 예제 코드

```txt
25-HTTP와-에러-처리/examples/http-error.html
```

## 실행 방법

1. `examples` 폴더를 엽니다.
2. `http-error.html`을 더블클릭합니다.
3. “불러오기” 버튼을 눌러 JSONPlaceholder에서 실제 데이터를 받아옵니다.
4. 콘솔(F12)에서 `try/catch`와 `instanceof Error` 출력을 확인합니다.

## 따라 해보기

`fetch`로 가져온 응답이 `404`일 때 콘솔에 “페이지를 찾을 수 없습니다”를 출력하는 코드를 작성해 보세요.

```js
const res = await fetch("https://jsonplaceholder.typicode.com/posts/9999");
if (!res.ok) {
  console.log("페이지를 찾을 수 없습니다");
}
```

## 자주 하는 실수

- `fetch`가 404/500을 자동으로 throw 한다고 믿고 `if (!res.ok)` 검사를 빠뜨림
- `catch (e)`에서 바로 `e.message`를 읽어서, `e`가 진짜 `Error`가 아닐 때 런타임 에러 발생
- 상태 코드를 안 보고 본문만 읽어서, 사실은 실패인 응답을 “성공”으로 처리

## 연습문제

1. HTTP 메서드 중 “생성”에 해당하는 것은?
2. 상태 코드 `404`는 어떤 의미인가요?
3. `fetch` 응답이 실패일 때 에러를 던지는 짧은 함수를 작성하세요.

## 정답 예시

```js
// 1. POST
// 2. 요청한 리소스(페이지)를 찾을 수 없음

async function getPost(id) {
  const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`);
  if (!res.ok) {
    throw new Error(`HTTP ${res.status}`);
  }
  return res.json();
}

try {
  const post = await getPost(1);
  console.log(post.title);
} catch (e) {
  console.log(e instanceof Error ? e.message : "알 수 없음");
}
```

## 요약

- HTTP 메서드는 **행위**(GET/POST/PUT/DELETE)를 나타냅니다.
- 상태 코드는 **결과**를 알려줍니다 (`2xx` 성공, `4xx` 내 잘못, `5xx` 서버 잘못).
- `try/catch/finally`로 실패에 대비하고, `instanceof Error`로 안전하게 메시지를 읽습니다.
- `fetch`는 `response.ok`를 직접 확인해 실패를 던져야 합니다.

## 다음 강의

[26강. 마무리 + React로 넘어가기](../26-마무리와-React-시작)