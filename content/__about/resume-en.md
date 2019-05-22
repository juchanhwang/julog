---
title: 'about'
date: 2019-1-27 16:21:13
lang: 'en'
---

# 👨‍💻 황주찬



<div align="center">
	<p>
    함께 협력하며 성장하기를 꿈꾸는 프론트엔지니어, 
    <code class=language-text>황주찬</code>입니다.<br>
    제가
    <code class=language-text>협력</code>과
    <code class=language-text>성장</code>의 가치를 알게 된 이야기를 소개해드리려 합니다.
	</p>
  <p> 
    비전공자로 JS언어를 '어떻게' 학습해야 하는지에 대한 어려움이 있었습니다.<br>
    여러 탐색 끝에 <code class=language-text>vanillaJS</code>로 학습하는 '코드스쿼드'에서 배우게 되었습니다.</p>
	<p>
  	JS언어의 개념적인 부분은 매주 마스터의 강의와, 스터디를 통해 극복했습니다.<br>
  	미니 프로젝트를 수행하면서 부딪치는 실질적인 코드작성에 대한 어려움은<br>
 		'구현'이라는 한 가지 목표를 가지고 부족한 기술적인 부분은 공식문서,<br>
	 	기술 블로그, 그리고 영상자료를 통해 학습하며 프로젝트를 마무리할 수 있었습니다.<br>
    <br>
    그 과정에서 마스터와의 코드리뷰를 통해 설계하는 방법과 좋은 코드가 무엇인지,<br>
    그리고 다른 개발자와 페어 코딩을 통해 '협업'하며 함께 '성장'하는 가치를 배울 수 있었습니다.<br>
  </p>
  <p>
  	이제는 실제 '서비스'와 '유저'를 경험하며 어떻게하면 좋은 개발자로<br>
    성장할 수 있을지를 고민하고 경험하는 개발자로 성장하고 싶습니다.
  </p>
</div>

<div align = center>
  <code class= language-text >#Javascript</code>
  <code class= language-text >#VanillaJS</code>
  <code class= language-text >#React</code> 
  <code class= language-text >#Front-End</code>
</div>





|            |                                |
| :--------: | ------------------------------ |
| **GitHub** | https://github.com/juchanhwang |
| **E-mail** | ghkdwncks96@gmail.com          |
| **Phone** | 010-9374-9106          |

# 💼 Work Experiences

## Code Squad

|             |                                                              |
| ----------: | ------------------------------------------------------------ |
|      period | 18.03 ~ 19.02                                                |
|   **major** | 웹 프론트엔드                                                |
|   **curriculum** | https://github.com/nigayo/front-end-curriculum/tree/2018_09                                               |
| description | - 윤지수 마스터님께  javscript, css, html 등 전반적인 프론트엔드 개발 교육을 받음<br><br> <br>- prototype, closure,  classes, Framework에 필요한 것들, ES2015이상의 문법, 함수형 메서드(forEach, filter, reduce.. )를 활용한 데이터처리 그리고,  DOM, Event,Ajax, Templating에 대한 원리와 특성 등 javascript와 그 외의 프론트앤드 개발자로서 필요한 전반적인 개발능력을 쌓기 위한 과정들을 공부했습니다.<br><br>개인 프로젝트로 다각형의 넓이 구하기, 학점 계산기 만들기, to-do list 기능 구현하기, Array Parser 만들기 등을 JS로 구현했습니다. 마지막 과정으로 순수 'VanillaJS'로 amazon.prime 웹서비스 클론 프로젝트를 진행했습니다.<br>- 모든 자바스크립트 개발자가 알아야 하는 33가지 개념이라는 주제로 스터디도  진행 중 <br>(https://github.com/33-js-concepts-study/JuChan.git) |







# 📊 Projects



## 아마존 사이트 클론

#### amazon.prime 웹서비스를 클론하는 프로젝트입니다

- 저장소 링크: https://github.com/juchanhwang/javascript-amazon
- 기술 스택: VanillaJS, JavaScript, HTML, CSS

#### Description

- 메인페이지 html, css 구조화 설계

- 상단 plans 레이어
  - 스크롤을 하면 화면상단에 검은색 bar가 새로 노출되고, 스크롤을 위쪽으로 올리면 다시 bar가 사라진다(scroll event )
  - 검은색 bar가 노출된 상태에서 ‘see more plans’ 영역을 클릭하면 두 번째 화면처럼 레이어가 애니메이션효과와 함께 펼쳐진다.
  - 펼쳐진 상태에서 접으려면, close 버튼을 누르던가, 하단의 화살표 모양을 누르면 다시 접힌다. 

- 캐러셀(Carousel) UI
  - 무한으로 동작하는 캐러셀(carousel) UI 구현
  - 총 4개의 panel이 있고, 3초 간격으로 애니메이션으로 동작한다(setInterval, transition 효과). 
  - 좌우 버튼을 누르면 즉시 이동되고, 일정 시간 동안 버튼 클릭이 없을 시, 자동이동이 시작된다.

    > 마지막 패널에서 처음 패널로 돌아올 때, 오른쪽으로 애니메이션이 동작하며 첫 패널로 돌아오게하는 기능을 구현하는 것에 있어서 어려움이 있었음. 첫 번째 패널 왼쪽에 4번째 패널을, 4번째 패널 오른쪽에 첫 번째 패널을 숨김으로써, 기능을 구현했음. 숨겨진 5번째 패널로 이동될 때, 애니메이션 동작을 주지 않은 상태로 1번째 패널로 이동하게 구현시켰음. 

- 검색 자동완성
  - 검색어를 입력하면 자동완성결과가 300ms 지연 후에 노출된다. (fecth API를 이용하여 JSON 데이터를 받아옴)
  - 입력창의 내용을 백스페이스로 삭제해도 일치하는 자동완성결과가 노출된다. 
  - 실제 검색버튼을 눌러도 검색이 이뤄지진 않으며, 자동완성 결과 창은 닫힌다. 

    > JSON데이터를 fetch API로 받아, 그 데이터를 다루는 작업에서 실질적으로 보이지 않는 데이터를 다루는 것이 어려웠음. 개발자 도구의 네트워크 탭에서 API를 받아오는 시점과 데이터에 접근하여 데이터를 조작하는 방법에 대해 이해를 할 수 있게 됨.



## Array Parser

#### javscript로 array형태의 데이터를 parsing하는 기능입니다.

- 저장소 링크: https://github.com/juchanhwang/javascript-json/tree/step7
- 기술 스택: JavaScript

#### Description

- 중첩 된 배열 형태의 문자열을 토큰 단위로 해석 후, 이를 분석하는 자료구조.

- testcase 작성

  > 함수의 동작이 예상에 맞게 동작하는지 테스트하는 것. 함수의 기능이 작든 크든 상관없이 테스트케이스를 작성할 수 있다는 것을 알게 되었다. 자료구조를 해석하고 다시 분석하며 발생할 수 있는 오류의 경우의 수들도 테스트함. 개발 당시에 오류가 없을 것이라 확신했던 코드에 오류가 없을 가능성이 적고, 오류가 날 상황을 직접 예측하고 대처할 수 있다는 것을, 작은 규모지만 경험할 수 있었다.



## 리액트로 만드는 무비 웹 

#### academy nomadcoder 강의를 통해 ReactJS를 학습했고, 결과물로 영화 및 드라마 스트리밍 서비스 홈 페이지를 제작했습니다

- 저장소 링크: https://github.com/juchanhwang/movie-app.git
- WebSite: https://juchans-movie.netlify.com/
- 기술 스택: ReactJS

### Description

- home, search, detail page UI 디자인
- themoviedb.com에서 제공하는 movieAPI와 tvAPI 로부터 데이터를  받아, 그 데이터들을 search하는 기능 또는, 구체적인 영화나 드라마 정보를 UI로 제공하는 기능 구현.



# 🎓 Education

## 충북 대학교

|            |                                            |
| ---------: | ------------------------------------------ |
|     period | 16.03 ~ (휴학)                             |
|  **major** | 경영 정보학(Management Information System) |
| **status** | 휴학                                       |

<div align="center" class="final">
감사합니다.
</div>