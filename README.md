# VUE를 이용하여 영화 사이트 만들기
![image](https://github.com/uUZINN/movie-project/assets/89904583/372be8c8-9f65-40e0-871d-7fbaabefa027)

VUE, TMBD API를 이용하여 나만의 영화 추천 사이트를 제작하였습니다.

## 설치
vue 설치<br>
`npm install -g @vue/cli`<br>
sass를 설치<br>
`npm install sass`<br>
react-router-dom 설치<br>
`npm install axios`<br>

### TMDB
https://www.themoviedb.org/?language=ko-KR<br>

TMDB API-Key 가져오기<br>

TMDB는 영화와 TV 프로그램에 대한 정보를 제공하는 온라인 데이터베이스입니다. <br>
영화, 드라마, 애니메이션 등의 작품 정보와 출연진, 제작진, 평점 등을 확인할 수 있습니다. <br>
개인적으로 영화를 찾거나 정보를 얻고 싶을 때 유용한 사이트 중 하나입니다.

### .env
.env 파일 생성<br>
.env 파일은 주로 환경 변수를 저장하는데 사용되는 파일 형식입니다. <br>
주로 프로젝트에서 사용되는 중요한 설정값들을 저장하고 보관하기 위해 사용됩니다.

### detail
1. 데이터 가져오기: fetchMovies 함수는 영화 정보를 API로부터 받아와서 movies에 저장합니다.<br>
   searchMovies 함수는 사용자가 입력한 검색어로 영화를 찾고 결과를 화면에 업데이트합니다.<br>
<br>
2. 화면 구성: 사용자가 선택한 '최신 영화', '인기 영화' 등의 탭을 클릭하면 해당하는 영화 정보가 화면에 나타납니다.<br>
   검색 기능은 검색어를 입력하고 '검색' 버튼을 클릭하거나 Enter 키를 누르면 작동합니다.<br>
<br>
3. 화면 디자인: CSS 부분은 검색창, 탭 메뉴, 영화 이미지 등의 디자인을 담당합니다.<br>
   Flexbox를 활용하여 영화 이미지들을 레이아웃에 맞추었습니다.<br>
<br>
TMDB API를 활용하여 Vue.js로 화면을 동적으로 업데이트하고, 사용자가 영화를 선택하거나 검색할 수 있도록 구현되어 있습니다.
