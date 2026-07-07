# Study Hub

> 코딩 학습 저장소 허브 — JavaScript 입문 + React/TypeScript 강의 노트

이 저장소는 두 가지 학습 과정을 **하나의 허브**로 묶어 관리합니다.

## 📚 학습 트랙

### 1. JavaScript 입문 강의 (`./javascript`)

코딩이 처음인 사람을 위한 JavaScript 기본 문법 학습 자료입니다.  
HTML 파일을 더블클릭해 바로 실행하면서 배우는 방식으로, 22강을 하루 1강씩 약 3주에 걸쳐 학습합니다.

| 강 | 주제 | 폴더 |
| --- | --- | --- |
| 00 | 환경설정 | [00-환경설정](./javascript/00-환경설정) |
| 01 | 프로그래밍이란 | [01-프로그래밍이란](./javascript/01-프로그래밍이란) |
| 02 | 변수와 상수 | [02-변수와상수](./javascript/02-변수와상수) |
| 03 | 자료형 | [03-자료형](./javascript/03-자료형) |
| 04 | 연산자 | [04-연산자](./javascript/04-연산자) |
| 05 | 조건문 | [05-조건문](./javascript/05-조건문) |
| 06 | 반복문 | [06-반복문](./javascript/06-반복문) |
| 07 | 함수 | [07-함수](./javascript/07-함수) |
| 08 | 배열 | [08-배열](./javascript/08-배열) |
| 09 | 객체 | [09-객체](./javascript/09-객체) |
| 10 | 배열과 객체 메서드 | [10-배열과객체-메서드](./javascript/10-배열과객체-메서드) |
| 11 | 스코프와 실행 흐름 | [11-스코프와실행흐름](./javascript/11-스코프와실행흐름) |
| 12 | DOM 기초 | [12-DOM기초](./javascript/12-DOM기초) |
| 13 | 이벤트 | [13-이벤트](./javascript/13-이벤트) |
| 14 | 폼과 입력값 | [14-폼과입력값](./javascript/14-폼과입력값) |
| 15 | 에러와 디버깅 | [15-에러와디버깅](./javascript/15-에러와디버깅) |
| 16 | JSON과 localStorage | [16-JSON과localStorage](./javascript/16-JSON과localStorage) |
| 17 | 비동기, Promise, fetch | [17-비동기-promise-fetch](./javascript/17-비동기-promise-fetch) |
| 18 | 모듈 | [18-모듈](./javascript/18-모듈) |
| 19 | npm 맛보기 | [19-npm맛보기](./javascript/19-npm맛보기) |
| 20 | 미니 프로젝트 | [20-미니프로젝트](./javascript/20-미니프로젝트) |
| 21 | 다음 단계 로드맵 | [21-다음단계-로드맵](./javascript/21-다음단계-로드맵) |

### 2. Web Front-End 기본 과정 (`./frontend_lecture`)

JavaScript 기초를 마친 후 듣는 **React + TypeScript 5일 강의** 노트입니다.  
출처: [26.07 / 삼성전자 Web Front-End 기본 과정](https://curse-battery-d1c.notion.site/26-07-Web-Front-End-390c672eb95e80d084eec11421e443ac) (박영웅 강사)

| 일차 | 주제 | 핵심 결과물 | 노트 |
| --- | --- | --- | --- |
| **Day 1** | 프로젝트 구성 + TypeScript 기초 + React 기본 문법 | Vite + React + TS 프로젝트, Counter 컴포넌트 | [📖 열기](./frontend_lecture/day1/README.md) |
| **Day 2** | React 컴포넌트 + CSS Module / Tailwind CSS + Context API | 재사용 컴포넌트, 다크모드 Context | [📖 열기](./frontend_lecture/day2/README.md) |
| **Day 3** | Zustand 상태 관리 + React Router 라우팅 기초 | Todo 상태 관리, 라우터로 페이지 분리 | [📖 열기](./frontend_lecture/day3/README.md) |
| **Day 4** | React Router 활용 + 네트워크/비동기 + App 개발 실습 1 | 영화 검색 앱 (OMDb API) | [📖 열기](./frontend_lecture/day4/README.md) |
| **Day 5** | App 개발 실습 2 + Vercel 배포 + 최종 평가 대비 | Todo 앱 (Heropy API) 배포 및 평가 정리 | [📖 열기](./frontend_lecture/day5/README.md) |

## 🗺️ 추천 학습 흐름

```txt
javascript/  (3주)
  ↓ JavaScript 기본 문법 + DOM/이벤트 + 비동기 + npm
frontend_lecture/day1/  ─┐
frontend_lecture/day2/   │  (5일)
frontend_lecture/day3/   │  React + TypeScript + 외부 라이브러리
frontend_lecture/day4/   │  라우팅 + 네트워크 + 실전 앱
frontend_lecture/day5/  ─┘
  ↓ Todo 앱을 Vercel에 배포하고 강의 종료
```

## 🔗 Git 일자 기록 규칙

각 학습 트랙은 **일자별 커밋**으로 진도를 남깁니다.

```bash
# JavaScript 강의 한 강을 끝낸 뒤
git add javascript/03-자료형/
git commit -m "docs(js-03): 자료형 강의 노트 추가"

# React 강의 한 일차를 끝낸 뒤
git add frontend_lecture/day3/
git commit -m "docs(day3): Zustand + React Router 기초 정리"
```

> 커밋 메시지 형식: `docs(트랙-식별자): 한 줄 요약`

## 📂 폴더 구조

```txt
study/
├── README.md                 ← 지금 보고 있는 파일 (허브)
├── .gitignore
├── javascript/               ← JS 입문 강의 (22강)
│   ├── 00-환경설정/
│   ├── ...
│   └── 21-다음단계-로드맵/
└── frontend_lecture/         ← React + TS 5일 강의 노트
    ├── day1/
    ├── day2/
    ├── day3/
    ├── day4/
    └── day5/
```

## 🔧 처음 클론한 후 설정

```bash
git clone https://github.com/Ranponim/study.git
cd study
# 학습 시작!
```

## 📖 참고 자료

- [강의 노션 페이지](https://curse-battery-d1c.notion.site/26-07-Web-Front-End-390c672eb95e80d084eec11421e443ac)
- [한눈에 보는 타입스크립트 (HEROPY.DEV)](https://www.heropy.dev/p/WhqSC8)
- [React 핵심 패턴 with TS (HEROPY.DEV)](https://www.heropy.dev/p/QduRma)
- [Tailwind CSS 핵심 패턴 (HEROPY.DEV)](https://www.heropy.dev/p/E67ZHS)
- [React Router 핵심 정리 (HEROPY.DEV)](https://www.heropy.dev/p/9tesDt)
- [Zustand 핵심 정리 (HEROPY.DEV)](https://www.heropy.dev/p/n74Tgc)
- [JS 비동기 핵심 패턴 (HEROPY.DEV)](https://www.heropy.dev/p/Zr4RfI)
- [Fetch vs Axios (HEROPY.DEV)](https://www.heropy.dev/p/QOWqjV)
- [강의 예제 저장소 (GitHub)](https://github.com/ParkYoungWoong/samsung-react-260706)