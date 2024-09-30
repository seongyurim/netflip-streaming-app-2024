![Header](https://capsule-render.vercel.app/api?type=rect&color=c5150c&text=NETFLIP&desc=TMDB%20데이터를%20활용한%20넷플릭스%20스타일의%20영화%20소개%20앱&section=header&height=250&fontColor=ffffff&fontSize=60&fontAlignY=45&descAlignY=67&descSize=30)
<br><br>

## 📍프로젝트 소개
TMDB 데이터를 활용하여 제작한 넷플릭스 스타일의 영화 소개 앱입니다. 영화를 주제별, 장르별로 확인하거나 제목으로 검색하여 취향에 맞는 작품을 탐색할 수 있습니다. 영화의 상세 정보와 예고편까지 사용자에게 제공합니다.

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

## 📍상세기능
### 1) Navigation
- 내비게이션은 단일 레이아웃 `AppLayout`으로 구성되었습니다.
- 따라서 모든 페이지 컴포넌트는 단일 레이아웃의 하위 라우트로 구성됩니다.
- 내비게이션 항목으로는 로고, 홈, 영화목록, 장르1, 장르2, 장르3 링크, 검색 버튼이 있습니다.

#### 1-1) 내비게이션 좌측: 페이지 이동 링크
#### 1-2) 내비게이션 우측: 검색 기능

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
	- 데이터가 있으면: 유튜브 컴포넌트를 이용해 예고편 동영상을 렌더링합니다.
	- 데이터가 없으면: 예고편 정보가 존재하지 않는다는 메세지를 렌더링합니다.
	- [유튜브 컴포넌트](https://www.npmjs.com/package/react-youtube)에는 API 호출로 받은 데이터의 key값을 전달하면 영상을 찾아와 보여줄 수 있습니다.

#### 2-3) MovieSlides: 여러 테마의 영화 리스트 슬라이드
- 수정중

### 3) Movie
### 4) MovieDetail
### 5) NotFound
