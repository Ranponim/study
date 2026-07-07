# 18강. 모듈

## 학습 목표

- JavaScript 파일을 나누는 이유를 이해합니다.
- `export`와 `import`의 기본 형태를 배웁니다.
- 모듈 실행 시 주의할 점을 압니다.

## 쉬운 설명

코드가 길어지면 한 파일에 모두 넣기 어렵습니다. 모듈은 코드를 여러 파일로 나누고 필요한 것만 가져와 쓰는 방법입니다.

## 내보내기

`message.js`:

```js
export const message = "모듈에서 온 메시지입니다.";

export function greet(name) {
  return name + "님, 안녕하세요!";
}
```

## 가져오기

`index.html` 안의 script:

```html
<script type="module">
  import { message, greet } from "./message.js";

  console.log(message);
  console.log(greet("민수"));
</script>
```

## 중요한 주의사항

모듈은 브라우저 보안 정책 때문에 일부 환경에서 파일 더블클릭으로 동작하지 않을 수 있습니다. 이 경우 VS Code의 Live Server 확장 또는 간단한 로컬 서버를 사용해야 합니다.

하지만 지금은 “파일을 나눌 수 있다”는 개념을 이해하는 것이 목표입니다.

## 예제 코드

예제 파일:

```txt
18-모듈/examples/index.html
18-모듈/examples/message.js
```

## 실행 방법

1. 먼저 `index.html`을 더블클릭해 봅니다.
2. 동작하지 않으면 VS Code Live Server로 실행합니다.
3. `message.js`의 문장을 바꾼 뒤 새로고침합니다.

## 자주 하는 실수

- `<script type="module">`을 빼먹음
- import 경로 앞의 `./`를 빼먹음
- 내보낸 이름과 가져오는 이름을 다르게 씀

## 연습문제

1. `message.js`에서 `today`라는 상수를 export하세요.
2. `index.html`에서 `today`를 import해 화면에 표시하세요.
3. 두 숫자를 더하는 함수를 export/import해 보세요.

## 정답 예시

```js
export function add(a, b) {
  return a + b;
}
```

```js
import { add } from "./message.js";
console.log(add(2, 3));
```

## 요약

- 모듈은 코드를 여러 파일로 나누는 방법입니다.
- `export`로 내보내고 `import`로 가져옵니다.
- 브라우저에서 모듈은 `type="module"`이 필요합니다.

## 다음 강의

[19강. npm 맛보기](../19-npm맛보기)
