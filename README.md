![Header](https://capsule-render.vercel.app/api?type=rect&color=c5150c&text=NETFLIP&desc=TMDB%20데이터를%20활용한%20넷플릭스%20스타일의%20영화%20소개%20앱&section=header&height=250&fontColor=ffffff&fontSize=60&fontAlignY=45&descAlignY=67&descSize=30)
<br><br>

## 📍프로젝트 소개
TMDB 데이터를 활용하여 제작한 넷플릭스 스타일의 영화 소개 앱입니다. 영화를 테마별, 장르별 혹은 제목으로 검색하여 취향에 맞는 작품을 탐색할 수 있습니다. 영화의 상세 정보와 예고편을 사용자에게 제공합니다.

## 📍개발기간
2024.08.20 ~ 09.06 (3주)

## 📍기술스택
<div>
	<img src="https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black">
	<img src="https://img.shields.io/badge/React Query-FF4154?style=for-the-badge&logo=reactquery&logoColor=white">
	<img src="https://img.shields.io/badge/React Router-CA4245?style=for-the-badge&logo=reactrouter&logoColor=white">
	<img src="https://img.shields.io/badge/CSS-1572B6?style=for-the-badge&logo=css3&logoColor=white"> 
	<img src="https://img.shields.io/badge/API Call-E3695F?style=for-the-badge&logoColor=white"> 
</div>

## 📍주요기능

## 📍폴더구성
<img width="1242" alt="Folder Tree" src="https://github.com/user-attachments/assets/c13e24a6-be9a-4b34-b47b-202f0ccdd0e4">


## 📍상세기능
### 1) Navigation
- 내비게이션은 단일 레이아웃 컴포넌트(`AppLayout`)으로 구성합니다.
- 따라서 모든 페이지 컴포넌트를 단일 레이아웃의 서브 라우트로 설정합니다.
- 내비게이션 항목으로는 로고, 홈, 영화목록, 장르1, 장르2, 장르3 링크와 검색 버튼이 있습니다.

#### 1-1) 내비게이션 좌측: 페이지 이동 링크
- 각 링크의 정보(링크명, URL)를 객체로 정의하여 관리합니다.
- 사용자가 현재 위치한 내비게이션 항목에만 CSS 선택자를 부여해서 강조합니다.
- 이를 위해 현재 위치 경로와 프로퍼티의 URL 정보를 비교하여 불리언 값을 도출합니다.
	- `useLocation`: `location.pathname`으로 현재 경로 반환
	- `URLSearchParams`: `new URLSearchParams(location.search).get()`으로 현재 쿼리 파라미터 값 반환
- 페이지 이동 설정은 react-router-dom의 `Link` 컴포넌트를 활용합니다.

#### 1-2) 내비게이션 우측: 검색 기능
- 영화를 제목으로 검색할 수 있습니다.
- 검색 아이콘을 클릭하면 아이콘 영역 위에 포커싱 된 검색창이 나타납니다.
- 이때 검색창은 `useRef`를 통해 요소가 마운트되면 포커싱되도록 설정합니다.
- 검색 키워드를 지역 상태로 저장하고, 이 상태를 URL에 반영하여 페이지를 이동시킵니다.

#### 1-3) Outlet: 서브 라우터 표시
- `AppLayout` 컴포넌트는 단일 내비게이션으로서 모든 페이지들을 서브 라우터로 가지고 있습니다.
- `AppLayout` 컴포넌트에서 별도로 명시하지 않으면 서브 라우터들은 렌더링되지 않습니다.
- 이를 위해 `react-router`의 `Outlet` 컴포넌트를 내비게이션의 최하단에 명시합니다.
- 이제는 내비게이션 아래에 서브 라우터들이 정상적으로 렌더링됩니다.
- 즉 `Outlet`을 사용하면 부모 라우트의 하위 라우트를 정의하고, 해당 하위 라우트에 매칭되는 컴포넌트를 렌더링할 수 있습니다.

### 2) Home
#### 2-1) Banner: 단일 영화 아이템 소개
- **`usePopularMoviesQuery`**: 인기영화 리스트를 가져옵니다.
- 리스트의 각 페이지에는 20개의 영화가 있고, 이중 하나를 랜덤으로 선택합니다.
- 선택된 영화의 제목, 포스터사진, 소개글, 장르를 가져와서 렌더링합니다.
- 링크로는 예고편과 상세정보가 있습니다.
	- 예고편: 선택된 영화의 예고편을 모달창을 통해 바로 시청할 수 있습니다.
	- 상세정보: 선택된 영화의 id가 URL 경로에 포함되어 상세정보 페이지로 이동합니다.

#### 2-2) PreviewModal: 유튜브 예고편 시청
- Banner → Preview 컴포넌트로 상태 전달
	- `movie`: 영화 정보 객체
	- `show`: `modalShow(false)`
	- `onHide`: `() => setModalShow(false)`
- **`useMoviePreviewQuery`**: movie의 id로 특정 영화의 예고편을 가져옵니다.
- `show`가 `true`인 경우 지역 상태 스위칭
	- `isMount`를 `true`로 변경: 컴포넌트를 조건부 렌더링하는 상태
	- `isVisible`를 `true`로 변경: 컴포넌트가 `isMount`에 의해 DOM에 추가된 뒤 트랜지션을 지연동작시키는 상태
- 모달창의 외부에 반투명 블랙 배경이 오버레이되도록 합니다. 오버레이를 클릭하면 모달이 사라집니다.
- API를 호출한 이후 데이터 유무에 따른 렌더링
	- 데이터가 있으면: 유튜브 컴포넌트(react-youtube)를 이용해 예고편 동영상을 렌더링합니다.
	- 데이터가 없으면: 예고편 정보가 존재하지 않는다는 메세지를 렌더링합니다.
	- [react-youtube](https://www.npmjs.com/package/react-youtube): API 호출로 받은 데이터의 key값을 전달하면 영상을 찾아와 보여줄 수 있습니다.

#### 2-3) MovieSlides: 여러 테마의 영화 리스트 슬라이드
- 인기 영화, 높은 평점 영화, 개봉 예정 영화, 현재 상영중인 영화 리스트를 보여줍니다.
- 홈 컴포넌트의 하위에서 각 슬라이드 컴포넌트를 생성하여 API를 호출하고 `MovieSlider` 컴포넌트로 데이터를 전달합니다.
- `MovieSlider` 컴포넌트에는 아래와 같은 속성을 전달하여 영화 리스트를 렌더링합니다.
	- `title`: 리스트의 제목
	- `movies`: 영화 정보 리스트
	- `responsive`: 슬라이더 반응형 객체
- 영화 슬라이더는 [react-multi-carousel](https://www.npmjs.com/package/react-multi-carousel)을 통해 구현합니다.
- 슬라이더를 구성하는 각 영화 정보들은 `MovieCard` 컴포넌트를 통해 렌더링됩니다.
	- `movie`: 단일 영화 정보
- 영화카드에는 포스터가 보이고, 포스터를 호버하면 간단한 영화 정보를 확인할 수 있습니다.
- 각 영화카드를 클릭하면 영화의 id가 URL에 반영되어 영화 정보 상세 페이지로 이동합니다.
- 각 영화의 데이터에는 영화의 장르 id가 있습니다.
	- id값에 매칭되는 문자열로 변경하여 렌더링합니다.
	- **`useMovieGenreQuery`**: 모든 장르 정보가 저장되어 있는 장르 객체를 가져옵니다.
 	- 특정 영화의 id값과 비교하여 문자열로 변환합니다.

### 3) Movies
#### 3-1) 사용자의 요청에 따라 동적 리스트 생성
- 영화 리스트 페이지로, 영화를 장르로 필터링하거나 검색 결과를 확인할 수 있습니다.
- `useSearchParams` 훅을 통해 현재 URL에서 검색어(`keyword`)와 장르 파라미터값(`genreParam`)을 각각 추출합니다.
- **`useSearchMediaQuery`**: 매개변수에 부합하는 영화 리스트를 가져옵니다.
	- `keyword`: 사용자가 입력한 검색어
	- `genre`: 사용자가 선택한 장르 혹은 URL에 명시된 장르
	- `page`: 영화 리스트의 페이지(20개 기준)
 - 이 커스텀 훅의 queryFn(`fetchSearchMovie`)에서는 키워드나 장르의 존재 여부에 따라 파라미터와 엔드포인트를 동적으로 설정합니다.
 - 단, 키워드와 장르 값이 모두 존재하지 않는다면 인기 영화 리스트를 디폴트 리스트로 보여줍니다.
```
const fetchSearchMovie = ({ keyword, genre, page }) => {
  const params = {
    language: 'ko',
    page,
    ...(keyword && { query: keyword }), // keyword가 있을 때에만 객체에 추가
    ...(genre && { with_genres: genre }), // genre가 있을 때에만 객체에 추가
  };

  const endpoint =
    keyword
      ? '/search/movie' // 키워드가 있는 경우
      : genre
        ? '/discover/movie' // 키워드가 없고, 장르만 있는 경우
        : '/movie/popular'; // 키워드와 장르가 모두 없는 경우

  return api.get(endpoint, { params });
};
```

#### 3-2) GenreSlider
- GenreSlider는 부모(`MoviePage`)의 상태를 전달받습니다.
	- `genreData`: 모든 장르의 id, name이 저장되어 있는 객체
	- `genreResponsive`: 캐러셀이 사용할 장르 슬라이더 전용 반응형 데이터
	- `selectedGenre`: 선택된 장르를 저장하는 상태
	- `setSelectedGenre`: 선택된 장르 설정
- 장르카드를 클릭하면 `useNavigate`를 통해 URL에 장르값을 반영합니다.
- 선택된 장르는 상태에 저장하고 CSS 선택자를 부여하여 UI를 강조합니다.
- 장르 슬라이더도 영화 슬라이더와 마찬가지로 react-multi-carousel를 통해 구현합니다.

#### 3-3) Pagination
- 영화 리스트의 페이지네이션은 [react-paginate](https://www.npmjs.com/package/react-paginate)를 사용하여 구현합니다.
- `page` 상태의 초기값을 1로 설정하고 `onPageChange` 함수를 통해 페이지가 1씩 증가하도록 합니다.

### 4) MovieDetail
- 영화 카드를 클릭하면 선택된 영화의 id가 URL 경로에 포함되어 상세정보 페이지로 이동합니다.
- 해당 페이지에는 영화의 상세정보뿐 아니라 비슷한 작품 추천, 리뷰도 포함되어 있습니다.

#### 4-1) DetailBanner
- **`useMovieDetailQuery`**: 영화의 id를 통해 영화 상세정보 객체를 가져옵니다.
- 상위 컴포넌트(`MovieDetail`)에서 위의 훅을 호출하여 데이터를 해당 컴포넌트에서 건네받습니다.
- 제목, 태그라인, 개봉일, 러닝타임, 평점, 좋아요수, 줄거리, 장르, 예고편, 포스터를 렌더링합니다.

#### 4-2) RelatedMoviesSlide
- **`useRelatedMoviesQuery`**: 영화의 id를 통해 해당 영화와 관련된 작품들을 가져옵니다.
- 영화 목록을 렌더링하기 위해 홈페이지에서 사용했던 MovieSlider 컴포넌트를 재사용합니다.

#### 4-3) Reviews
- **`useMovieReviewQuery`**: 영화의 id를 통해 관람객의 리뷰 정보들을 가져옵니다.
- 리뷰 목록을 렌더링하기 위해 `ReviewBox` 컴포넌트에 리뷰 데이터를 전달합니다.
- 데이터가 없으면 리뷰 정보가 없다는 안내문구를 렌더링합니다.

#### 4-4) ReviewBox
- 프로필사진: 리뷰어의 프로필사진이 없으면 디폴트 이미지를 보여줍니다.
- 닉네임: 리뷰어의 닉네임을 가져옵니다.
- 작성일: ISO 형식의 날짜(2024-06-06T19:47:54.037Z)를 커스텀(2024-06-06 19:47)합니다.
- 리뷰: 내용이 길면(단어가 50개 이상이면) 내용을 줄이고 더보기/접기 버튼을 보여줍니다.
- 평점: 평점(1~10)만큼 별을 렌더링하는 함수를 통해 정보를 효과적으로 시각화합니다.
	- `fullStars`: 평점을 2로 나누고, 나머지는 버립니다.
	- `hasHalfStar`: 평점을 2로 나머지 연산했을 때 1이면 `true`를 반환합니다.
	- `stars`: 다섯 개의 별이 들어갈 빈 배열을 만듭니다.
	- 5번을 순회하는 반복문을 만듭니다.
	- `i`가 `fullStars`보다 작으면: 노란 별을 만들어 `stars`에 추가합니다.
	- `i`가 `fullStars`와 같고 `hasHalfStar`가 `true`이면: 반은 노랗고 반은 회색인 별을 만들어 `stars`에 추가합니다.
	- 나머지 경우: 회색 별을 만들어 `stars`에 추가합니다.
	- 이로써 다섯 개의 별을 모두 만들었으니, stars를 반환합니다.

### 5) NotFound
- 사용자가 존재하지 않는 페이지를 요청했을 때 표시되는 404 NotFound 페이지입니다.
- 사용자가 잘못된 URL로 접근했을 때 일반적인 오류 화면보다 친숙한 NETFLIP만의 디자인으로 안내함으로써 사용자가 혼란스러워하지 않도록 돕습니다.
- 또한 홈페이지로 이동하는 링크를 만들고, 배경과 하단 문구로 인기영화를 랜덤으로 소개함으로써 컨텐츠에 대한 사용자의 흥미를 이끌어내 이탈률을 줄입니다.
- **`usePopularMoviesQuery`**: 인기영화 리스트를 가져오고, 그 뒤에 랜덤으로 하나만 선택되게 합니다.
- 이로써 웹사이트의 신뢰도와 완성도를 향상시킵니다.

### 5) 기타 컴포넌트
- LoadingSpinner: 로딩스피너를 keyframes로 직접 구현합니다.
- Footer: 상업용 페이지의 푸터를 참고하여 레이아웃을 설계하고 반응형으로 구현합니다.
