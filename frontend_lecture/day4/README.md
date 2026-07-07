# Day 4. React Router 활용 + 네트워크/비동기 + App 개발 실습 1 (영화 검색)

## 학습 목표

Day 4가 끝나면 다음을 할 수 있습니다.

- [ ] `useSearchParams`로 **쿼리스트링**을 읽고/바꾸는 검색 페이지를 만들 수 있다.
- [ ] `useNavigate`에 **state**를 실어 보내고, 받는 페이지에서 꺼낼 수 있다.
- [ ] **fetch**와 **Axios**의 차이를 알고, Axios로 API를 호출할 수 있다.
- [ ] **OMDb API**로 영화를 검색하고, 결과를 **로딩/에러/성공** 상태로 렌더링할 수 있다.

---

## 1. React Router 심화

Day 3에서 `<Link>`, `useNavigate`, `useParams`를 배웠습니다.  
Day 4에서는 **검색어(query string)**와 **state**를 다뤄 봅니다.

### 1.1 `useSearchParams` — 쿼리스트링 다루기

URL에 `?q=어벤져스&page=2`처럼 데이터를 실어 나르는 방식입니다.  
검색 결과 페이지, 페이지네이션, 필터 등에서 흔히 쓰입니다.

```tsx
// /search?q=어벤져스&page=2 → q="어벤져스", page="2"
import { useSearchParams } from 'react-router-dom'

function SearchPage() {
  const [searchParams, setSearchParams] = useSearchParams()

  const query = searchParams.get('q') ?? ''   // 현재 검색어
  const page  = searchParams.get('page') ?? '1'

  return (
    <div className="p-4">
      <p>검색어: {query}, 페이지: {page}</p>

      <button
        onClick={() => setSearchParams({ q: '인터스텔라', page: '1' })}
        className="mt-2 px-3 py-1 bg-blue-500 text-white rounded"
      >
        "인터스텔라"로 검색
      </button>
    </div>
  )
}
```

흐름:

```txt
1) 사용자가 버튼 클릭
2) setSearchParams({ q: '인터스텔라', page: '1' })
3) URL이 /search?q=인터스텔라&page=1 로 바뀜 (새로고침 X)
4) 컴포넌트가 자동 재렌더링되며 searchParams에서 새 값을 읽음
```

### 1.2 `useNavigate`에 state 실어 보내기

URL에 노출하면 안 되는 데이터(예: 결제 정보, 임시 폼 입력)는 state로 보냅니다.

```tsx
// 보내는 쪽
import { useNavigate } from 'react-router-dom'

const navigate = useNavigate()
navigate('/todo/3', { state: { from: 'home', note: '급해요' } })
```

```tsx
// 받는 쪽
import { useLocation } from 'react-router-dom'

function TodoDetail() {
  const location = useLocation()
  const state = location.state as { from?: string; note?: string } | null
  return <p>note: {state?.note ?? '(없음)'}</p>
}
```

> ⚠️ state는 **새로고침하면 사라집니다.** 영구 저장은 URL 쿼리나 store를 쓰세요.

### 1.3 언제 무엇을 쓰는가?

| 방식 | URL에 보임 | 새로고침 후 유지 | 적합한 데이터 |
| --- | --- | --- | --- |
| **Path 파라미터** (`/todo/:id`) | ✅ | ✅ | 리소스 식별자 |
| **Query** (`?q=...&page=2`) | ✅ | ✅ | 검색어, 필터, 페이지 |
| **State** | ❌ | ❌ | 결제/임시 데이터, 모달 |

---

## 2. 네트워크 통신과 비동기

### 2.1 이미 아는 것 — `fetch` + `async/await`

JavaScript 17강에서 `fetch`로 데이터를 받아오는 법을 배웠습니다. (참고: `../javascript/17-비동기-promise-fetch`)

```ts
const response = await fetch('https://example.com/api/data')
const data = await response.json()
```

여기서 한 단계 더 — **fetch는 HTTP 에러(404, 500)를 자동으로 throw하지 않는다**는 점을 기억하세요.

```ts
async function getData() {
  const res = await fetch('/api/data')
  if (!res.ok) throw new Error(`HTTP ${res.status}`)
  return res.json()
}
```

### 2.2 Axios — 더 편한 HTTP 클라이언트

Axios는 fetch를 감싸서 **자동 JSON 변환 + 에러 throw**를 해주는 라이브러리입니다. (참고: [Fetch vs Axios](https://www.heropy.dev/p/QOWqjV))

```bash
npm install axios
```

**fetch vs Axios 비교**

```ts
// fetch
const res = await fetch('https://omdbapi.com/?apikey=7035c60c&s=avengers')
if (!res.ok) throw new Error('에러!')
const data = await res.json()   // 수동으로 .json()

// axios
import axios from 'axios'
const { data } = await axios.get('https://omdbapi.com/', {
  params: { apikey: '7035c60c', s: 'avengers' },
})
// data는 이미 객체 + 4xx/5xx는 자동으로 throw
```

> 💡 강의에서는 **Axios를 표준**으로 씁니다. 인터셉터(요청/응답 가로채기), 기본 URL 설정 등 부가 기능이 풍부하기 때문입니다.

### 2.3 API 모듈 분리 — 깔끔하게 관리하기

API 호출 코드를 컴포넌트 안에 직접 쓰면, 컴포넌트가 너무 뚱뚱해집니다.  
별도 파일로 빼두면 재사용과 테스트가 쉬워집니다.

```ts
// src/api/instance.ts — axios 인스턴스
import axios from 'axios'

export const api = axios.create({
  baseURL: 'https://omdbapi.com/',
  params: { apikey: '7035c60c' },   // 공통 파라미터
})
```

```ts
// src/api/movie.ts — 영화 검색 함수
import { api } from './instance'

export interface MovieSummary {
  Title: string
  Year: string
  imdbID: string
  Type: string
  Poster: string
}

interface SearchResponse {
  Search?: MovieSummary[]
  totalResults?: string
  Response: 'True' | 'False'
  Error?: string
}

export async function searchMovies(keyword: string): Promise<MovieSummary[]> {
  const { data } = await api.get<SearchResponse>('', {
    params: { s: keyword },
  })
  if (data.Response === 'False') {
    throw new Error(data.Error ?? '검색 실패')
  }
  return data.Search ?? []
}
```

> OMDb API는 검색 결과를 `Search` 배열로 돌려주고, 결과가 없으면 `Response: "False"` + `Error` 메시지를 줍니다.  
> 타입을 미리 적어두면 컴포넌트에서 자동완성이 작동합니다. (TypeScript의 진짜 위력!)

---

## 3. React에서 비동기 다루기 — 3가지 상태

API를 호출할 때는 항상 **3가지 상태**를 함께 관리합니다.

```ts
type AsyncState<T> =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: T }
  | { status: 'error'; error: string }
```

직접 작성하면 번거로우니, 강의에서는 **Zustand store에 함께 보관**합니다.

```ts
// src/stores/useMovieStore.ts
import { create } from 'zustand'
import { searchMovies, type MovieSummary } from '../api/movie'

type Status = 'idle' | 'loading' | 'success' | 'error'

interface MovieState {
  keyword: string
  status: Status
  error: string
  movies: MovieSummary[]
  setKeyword: (v: string) => void
  search: (keyword: string) => Promise<void>
}

export const useMovieStore = create<MovieState>((set) => ({
  keyword: '',
  status: 'idle',
  error: '',
  movies: [],
  setKeyword: (v) => set({ keyword: v }),
  search: async (keyword) => {
    set({ status: 'loading', error: '' })
    try {
      const movies = await searchMovies(keyword)
      set({ status: 'success', movies })
    } catch (e) {
      const msg = e instanceof Error ? e.message : '알 수 없는 오류'
      set({ status: 'error', error: msg, movies: [] })
    }
  },
}))
```

`useEffect`로 URL의 쿼리를 읽어 자동으로 검색을 트리거합니다.

```tsx
// src/pages/Movies.tsx
import { useEffect } from 'react'
import { useSearchParams, useNavigate } from 'react-router-dom'
import { useMovieStore } from '../stores/useMovieStore'

export default function Movies() {
  const [params] = useSearchParams()
  const navigate = useNavigate()
  const q = params.get('q') ?? ''

  const keyword    = useMovieStore((s) => s.keyword)
  const setKeyword = useMovieStore((s) => s.setKeyword)
  const search     = useMovieStore((s) => s.search)
  const status     = useMovieStore((s) => s.status)
  const error      = useMovieStore((s) => s.error)
  const movies     = useMovieStore((s) => s.movies)

  // URL의 q가 바뀌면 자동 검색
  useEffect(() => {
    if (q) {
      setKeyword(q)
      search(q)
    }
  }, [q, search, setKeyword])

  const onSubmit = (e: React.FormEvent) => {
    e.preventDefault()
    navigate(`/movies?q=${encodeURIComponent(keyword)}`)
  }

  return (
    <div className="max-w-3xl mx-auto p-4">
      <h1 className="text-2xl font-bold mb-4">🎬 영화 검색</h1>

      <form onSubmit={onSubmit} className="flex gap-2">
        <input
          className="border px-2 py-1 rounded flex-1"
          value={keyword}
          onChange={(e) => setKeyword(e.target.value)}
          placeholder="영화 제목 입력"
        />
        <button className="bg-blue-500 text-white px-3 py-1 rounded">
          검색
        </button>
      </form>

      {/* 상태별 렌더링 */}
      <div className="mt-6">
        {status === 'loading' && <p className="text-gray-500">검색 중...</p>}
        {status === 'error'   && <p className="text-red-500">{error}</p>}
        {status === 'success' && movies.length === 0 && (
          <p className="text-gray-500">결과가 없어요.</p>
        )}
        {status === 'success' && movies.length > 0 && (
          <ul className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-4">
            {movies.map((m) => (
              <li key={m.imdbID} className="border rounded p-2">
                <img
                  src={m.Poster !== 'N/A' ? m.Poster : '/placeholder.png'}
                  alt={m.Title}
                  className="w-full aspect-[2/3] object-cover"
                />
                <h2 className="mt-2 font-semibold text-sm">{m.Title}</h2>
                <p className="text-xs text-gray-500">{m.Year}</p>
              </li>
            ))}
          </ul>
        )}
      </div>
    </div>
  )
}
```

### 3.1 `useEffect` 기본

```ts
useEffect(() => {
  // 실행할 코드
  console.log('q가 바뀌었다!')
}, [q])   // q가 바뀔 때만 다시 실행
```

- 의존성 배열(`[q]`)에 들어간 값이 **바뀔 때마다** 안의 함수가 실행됩니다.
- 의존성 배열을 비우면(`[]`) 마운트 시 한 번만 실행됩니다.
- `useEffect` 안에서 직접 `await`할 수 있지만, 보통은 store action으로 빼서 처리합니다 (테스트하기 쉽도록).

> 🚨 `useEffect`에서 직접 state 변경을 하지 말라고 경고가 뜨면, 보통 store에 빼라는 신호입니다.

---

## 4. 실습 — 영화 검색 앱 (OMDb API)

위 코드를 `src/pages/Movies.tsx`에 그대로 넣고, 라우터를 추가합니다.

```tsx
// src/App.tsx
import { Routes, Route } from 'react-router-dom'
import Header from './components/Header'
import Home from './pages/Home'
import About from './pages/About'
import Todo from './pages/Todo'
import Movies from './pages/Movies'
import NotFound from './pages/NotFound'

export default function App() {
  return (
    <div>
      <Header />
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/todo" element={<Todo />} />
        <Route path="/movies" element={<Movies />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </div>
  )
}
```

`Header`에 `/movies` 링크도 추가:

```tsx
<Link to="/movies" className="hover:underline">Movies</Link>
```

### 4.1 OMDb API 키

강의 노션에서 제공한 키를 그대로 씁니다. (`apikey=7035c60c`)  
본인 키를 발급받으려면 [omdbapi.com](https://www.omdbapi.com/apikey.aspx)에서 무료로 받을 수 있습니다.

**API 호출 예시 (브라우저에서 직접 확인해 보기)**

```txt
# 검색어로 영화 목록
https://omdbapi.com/?apikey=7035c60c&s=avengers

# IMDb ID로 상세 정보
https://omdbapi.com/?apikey=7035c60c&i=tt4154796
```

### 4.2 자주 보는 OMDb 응답

```json
// 검색 결과 (s=avengers)
{
  "Search": [
    {
      "Title": "Avengers: Endgame",
      "Year": "2019",
      "imdbID": "tt4154796",
      "Type": "movie",
      "Poster": "https://m.media-amazon.com/images/M/...jpg"
    }
  ],
  "totalResults": "344",
  "Response": "True"
}

// 결과 없음
{
  "Response": "False",
  "Error": "Movie not found!"
}
```

---

## 5. 자주 하는 실수 체크리스트

- [ ] `useEffect` 의존성 배열에 객체를 넣어서 매번 실행됨 → 원시값/함수 ref만 넣기
- [ ] `setState`를 `await` 안에서 호출하고 다음 줄에서 그 값을 읽음 → state는 즉시 반영되지 않음 (closure 문제)
- [ ] API 키를 코드에 하드코딩 → 실서비스에서는 환경변수(`import.meta.env.VITE_...`)로 분리
- [ ] `useNavigate`에 한글 검색어 그대로 넣음 → `encodeURIComponent` 필수
- [ ] `await`를 빼먹어서 `data`가 Promise 객체로 남음

---

## 6. 연습 문제

### 문제 1. 디바운싱 검색

입력할 때마다 검색하면 API 호출이 너무 많습니다. **500ms 동안 추가 입력이 없으면 검색**하도록 만들어 보세요. (힌트: `setTimeout` + cleanup)

```ts
useEffect(() => {
  const timer = setTimeout(() => navigate(`/movies?q=${keyword}`), 500)
  return () => clearTimeout(timer)  // cleanup: 다음 effect 실행 전에 이전 거 취소
}, [keyword])
```

### 문제 2. 상세 페이지

영화 카드를 클릭하면 `/movies/:imdbID`로 이동하고, 상세 정보를 보여주는 `MovieDetail` 페이지를 만들어 보세요. (힌트: `useParams`, Axios로 `?i=` 호출)

### 문제 3. 에러 폴백 UI

검색 결과가 없을 때(`success`인데 `movies.length === 0`) 다른 영화 추천 메시지를 보여주는 `EmptyState` 컴포넌트를 분리해 보세요.

---

## 7. 요약

| 개념 | 한 줄 |
| --- | --- |
| **`useSearchParams`** | URL의 쿼리스트링 읽기/쓰기 |
| **`useNavigate` state** | 새로고침에 사라지는 임시 데이터 전달 |
| **`fetch` vs `axios`** | fetch는 수동 JSON, axios는 자동 + 에러 throw |
| **3가지 비동기 상태** | loading / success / error |
| **`useEffect`** | 의존성 배열에 명시한 값이 바뀔 때 실행 |
| **API 모듈 분리** | `api/instance.ts` + 도메인별 함수 |

---

## 8. 다음 단계

Day 5에서는 Day 4의 Todo 앱을 **Heropy API와 연결**해 진짜 서버 데이터로 만들고, **Vercel**로 배포합니다.

👉 [Day 5 노트 열기 — App 개발 실습 2 + Vercel 배포 + 최종 평가 대비](./../day5/README.md)