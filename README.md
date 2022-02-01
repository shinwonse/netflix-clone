## 설맞이 넷플릭스 클론

### 1. 라우팅 설정, 테마 설정

`react-router-dom` 과 `styled-components` 로 진행

### 2. 헤더 구현
![화면 기록 2022-02-01 오후 8 59 38](https://user-images.githubusercontent.com/62709718/151964986-e22d8fc1-e15f-45cb-b48a-005d4f2f891c.gif)

`framer-motion` 을 이용해 애니메이션을 입힘 (스크롤에 따른 헤더 변화, 검색 버튼 누름 여부에 따른 스타일, 버튼 이동 등)

### 3. 홈 화면
`react-query` 를 사용. `react-query` 의 사용법을 다시 복습할 수 있었던 기회.

***src/api.ts***
```typescript
const API_KEY = "2ba38dbd419e938d0ac1a4b4f4219034";
const BASE_PATH = "https://api.themoviedb.org/3";

export function getMovies() {
  return fetch(`${BASE_PATH}/movie/now_playing?api_key=${API_KEY}`).then(
    (response) => response.json()
  );
}

```
***themoviedb.org*** 의 api를 이용하여 api.ts 파일 생성.
***src/Routes/Home.tsx***
```typescript
import { useQuery } from "react-query";
import { getMovies } from "../api";

function Home() {
  const { data, isLoading } = useQuery(["movies", "nowPlaying"], getMovies);
  console.log(data, isLoading);
  return (
    <div style={{ backgroundColor: "whitesmoke", height: "200vh" }}>home</div>
  );
}
export default Home;
```
위와 같은 식으로 `react-query` 사용. `useQuery` 로 받아오는 결과의 첫번째는 결과 데이터, 두번째는 로딩 여부.

Typescript를 사용하기 위해서 우리는 API 응답의 타입을 정해줘야함.
```typescript
interface IMoive {
  id: number;
  backdrop_path: string;
  poster_path: string;
  title: string;
  overview: string;
}

export interface IGetMoviesResult {
  dates: {
    maximum: string;
    minimum: string;
  };
  page: number;
  results: [];
  total_pages: number;
  total_results: number;
}
```
이후 css 작업을 통해 홈 화면을 구성하였는데 이 때 처음 해보는 과정이 있었음. 바로 백그라운드 이미지에 두가지 속성값을 넣는 것이었음
```typescript
const Banner = styled.div<{ bgPhoto: string }>`
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 60px;
  background-image: linear-gradient(rgba(0, 0, 0, 0), rgba(0, 0, 0, 1)),
    url(${(props) => props.bgPhoto});
  background-size: cover;
`;
```
이런 식으로 `,` 을 통해 두가지 속성값을 넣을 수 있었음

<img width="1390" alt="스크린샷 2022-02-01 오후 10 29 05" src="https://user-images.githubusercontent.com/62709718/151977104-eab0f93c-76e8-496b-95e2-eff1ff7927e7.png">

### 4. 슬라이더 구현
`framer-motion` 을 이용하여 구현. 자주 쓰게 되는 함수의 경우 `utils.ts` 에서 따로 관리. 슬라이더에 6개씩 이미지를 띄우기 위해 사용한 로직을 살펴볼 필요가 있음

![화면 기록 2022-02-01 오후 11 43 52](https://user-images.githubusercontent.com/62709718/151989972-a07972e7-5457-4d19-a7d4-9e10e9a32e07.gif)

