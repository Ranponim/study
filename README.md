# Frontend Lecture Notes

> 26.07 / 삼성전자 Web Front-End 기본 과정 (박영웅 강사) - 5일 학습 노트

이 저장소는 **React + TypeScript 입문 강의** 5일 과정을 처음 듣는 사람 기준으로 정리한 학습 노트입니다.  
본문의 출처는 노션 강의 페이지이며, 각 일차별 핵심 개념을 **미니 프로젝트/예제 흐름**으로 풀어서 설명합니다.

## 학습 로드맵

| 일차 | 주제 | 핵심 결과물 |
| --- | --- | --- |
| [Day 1](./day1/README.md) | 프로젝트 구성 + TypeScript 기초 + React 기본 문법 | Vite + React + TS 프로젝트 띄우기, `Counter` 컴포넌트 작성 |
| [Day 2](./day2/README.md) | React 컴포넌트 + CSS Module / Tailwind CSS + Context API | 재사용 컴포넌트, 테마 전환 Context, Tailwind 버튼/모달 |
| [Day 3](./day3/README.md) | Zustand 상태 관리 + React Router 기초 | Zustand 스토어로 Todo 상태 관리, 라우터로 페이지 분리 |
| [Day 4](./day4/README.md) | React Router 활용 + 네트워크/비동기 + App 개발 실습 1 | 영화 검색 앱 (OMDb API) |
| [Day 5](./day5/README.md) | App 개발 실습 2 + Vercel 배포 + 최종 평가 대비 | Todo 앱 (Heropy API) 배포 및 평가 정리 |

## 사전 지식 (이미 알고 있다고 가정)

본 노트는 다음을 **이미 알고 있다고 가정**합니다. 모르는 부분이 있으면 옆 저장소를 먼저 보세요.

- **JavaScript 기본**: 변수/상수, 자료형, 연산자, 조건문, 반복문, 함수, 배열, 객체  
  → `../javascript/01-프로그래밍이란` ~ `../javascript/10-배열과객체-메서드`
- **DOM/이벤트**: `document.querySelector`, `addEventListener`  
  → `../javascript/12-DOM기초`, `../javascript/13-이벤트`
- **비동기**: `async/await`, `fetch`, `try/catch`  
  → `../javascript/17-비동기-promise-fetch`
- **모듈**: `import` / `export`  
  → `../javascript/18-모듈`
- **npm**: `npm install`, `npm run dev` 정도는 알고 있다고 가정 (자세한 설명은 생략)
- **Git**: `add` / `commit` / `push` 기본 사용 가능  
  → 본 노트는 `git` 흐름을 일자별로 정리합니다.

> Tip: "변수 선언만 안다"고 하셨지만, 이미 JavaScript 22강을 모두 학습하셨습니다. **새로운 패러다임** (TS 타입 / 컴포넌트 / 상태)에만 집중하면 됩니다.

## 학습 곡선 설계

5일 과정은 분량이 많기 때문에, 노트는 다음 원칙으로 작성했습니다.

1. **기존 JS 지식은 빠르게 참조** - `let/const`, `function`, `fetch` 등은 "이미 알고 있는 내용"으로 짧게 짚고 넘어갑니다.
2. **새로운 개념은 작게 쪼개서 단계적으로** - 한 번에 5개 개념을 섞지 않고, **하나의 개념 → 작은 예제 → 핵심 요약** 흐름으로 갑니다.
3. **실행 가능한 코드 우선** - 모든 예제는 `npm create vite@latest`로 만든 프로젝트에 그대로 붙여 넣으면 동작합니다.
4. **그림으로 상상하기** - 컴포넌트 트리, 상태 흐름 같은 추상적인 개념은 ASCII 다이어그램으로 시각화합니다.
5. **에러 메시지를 친구처럼** - 자주 보이는 에러는 별도 박스로 모아 두었습니다. 무서워하지 마세요.

## 폴더 구조

```txt
frontend_lecture/
├── README.md           ← 지금 보고 있는 파일
├── .gitignore
└── day1/ ~ day5/
    └── README.md       ← 일자별 강의 노트
```

각 일차 노트는 다음 순서로 읽으면 됩니다.

1. **학습 목표** - 오늘 끝나면 무엇을 할 수 있는가
2. **핵심 개념** - "왜 필요한가"부터 짧게
3. **코드 예제** - 직접 따라 치는 작은 예제
4. **자주 하는 실수** - 막힐 때 보는 체크리스트
5. **연습 문제** - 스스로 풀어보는 미니 과제
6. **요약 / 다음 단계** - 한 줄 정리 + 다음 일차 링크

## 커밋 규칙 (Git 일자 기록)

본 노트는 **일자별로 커밋**해서 학습 진도를 git history에 남깁니다.

```bash
# day1 작업 후
git add day1/
git commit -m "docs(day1): 프로젝트 구성 + TypeScript + React 기본 정리"

# day2 작업 후
git add day2/
git commit -m "docs(day2): 컴포넌트 + Tailwind + Context API 정리"
```

> 커밋 메시지 형식: `docs(dayN): 한 줄 요약`

## 참고 자료 (강의 노션에 링크된 글)

- [한눈에 보는 타입스크립트 (HEROPY.DEV)](https://www.heropy.dev/p/WhqSC8)
- [React 프로젝트 시작하기 w. Vite (HEROPY.DEV)](https://www.heropy.dev/p/6iFzkB)
- [React 핵심 패턴 with TS (HEROPY.DEV)](https://www.heropy.dev/p/QduRma)
- [Tailwind CSS 핵심 패턴 (HEROPY.DEV)](https://www.heropy.dev/p/E67ZHS)
- [React Context API 핵심 정리 (HEROPY.DEV)](https://www.heropy.dev/p/EdhHX2)
- [React Router 핵심 정리 (HEROPY.DEV)](https://www.heropy.dev/p/9tesDt)
- [Zustand 핵심 정리 (HEROPY.DEV)](https://www.heropy.dev/p/n74Tgc)
- [JS 비동기 핵심 패턴 (HEROPY.DEV)](https://www.heropy.dev/p/Zr4RfI)
- [Fetch vs Axios (HEROPY.DEV)](https://www.heropy.dev/p/QOWqjV)
- [강의 예제 저장소 (GitHub)](https://github.com/ParkYoungWoong/samsung-react-260706)

> 본 노트는 위 자료를 **입문자 관점에서 재구성**한 것입니다. 강의가 끝난 뒤 위 글들을 다시 읽으면 한층 깊어집니다.

## 시작하기

👉 [Day 1 노트 열기](./day1/README.md)