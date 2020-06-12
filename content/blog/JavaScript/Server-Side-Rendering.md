---
title: '서버 사이드 랜더링(SSR)'
date: 2020-06-12 16:00:00
category: 'JavaScript'

---



## 서버 사이드 랜더링(SSR)

- 전통적인 웹 애플리케이션은 서버 사이드 렌더링 방식
- 요청시마다 서버에서 처리한 후 새로고침으로 페이지에 대한 응답(view)
- 웹에서 많은 정보와 기능이 많아지면서 spa개념이 생김

![ssr](https://user-images.githubusercontent.com/36187948/84455530-871c3280-ac98-11ea-8c0c-6748da88301e.png)



## 클라이언트 사이드 렌더링(CSR)

- 클라이언트에서 자바스크립트를 통해 렌더링 하는 방식
- SPA는 클라이언트 사이드 렌더링 방식

![csr](https://user-images.githubusercontent.com/36187948/84455534-87b4c900-ac98-11ea-828f-36c2570a4c0a.png)



### CSR vs SSR

![csrVsSsr](https://user-images.githubusercontent.com/36187948/84455926-a1a2db80-ac99-11ea-8db5-aa50b36fab05.png)



### Single Page Application(SPA)

단일 페이지 애플리케이션(Single Page Application, SPA)는 모던 웹의 패러다임이다. SPA는 기본적으로 단일 페이지로 구성되며 기존의 서버 사이드 렌더링과 비교할 때, 배포가 간단하며 네이티브 앱과 유사한 사용자 경험을 제공할 수 있다는 장점이 있다.

link tag를 사용하는 전통적인 웹 방식은 새로운 페이지 요청 시마다 정적 리소스가 다운로드되고 전체 페이지를 다시 렌더링하는 방식을 사용하므로 새로고침이 발생되어 사용성이 좋지 않다. 그리고 변경이 필요없는 부분를 포함하여 전체 페이지를 갱신하므로 비효율적이다.

SPA는 기본적으로 웹 애플리케이션에 필요한 모든 정적 리소스를 최초에 한번 다운로드한다. 이후 새로운 페이지 요청 시, 페이지 갱신에 필요한 데이터만을 전달받아 페이지를 갱신하므로 전체적인 트래픽을 감소할 수 있고, 전체 페이지를 다시 렌더링하지 않고 변경되는 부분만을 갱신하므로 새로고침이 발생하지 않아 네이티브 앱과 유사한 사용자 경험을 제공할 수 있다.

모바일의 사용이 증가하고 있는 현 시점에 트래픽의 감소와 속도, 사용성, 반응성의 향상은 매우 중요한 이슈이다. SPA의 핵심 가치는 **사용자 경험(UX) 향상**에 있으며 부가적으로 애플리케이션 속도의 향상도 기대할 수 있어서 모바일 퍼스트(Mobile First) 전략에 부합한다.



## 서버 사이드 렌더링은 왜 필요한가요?

1. [검색 엔진 최적화(SEO)](https://static.googleusercontent.com/media/www.google.com/en//webmasters/docs/search-engine-optimization-starter-guide.pdf)를 통해 웹 크롤러에 대응하기 위해
2. 모바일 장비와 저사양 장비에서 동작하는 성능을 끌어올리기 위해
3. [사용자에게 유효한 첫 페이지](https://developers.google.com/web/tools/lighthouse/audits/first-contentful-paint)를 빠르게 표시하기 위해



### 웹 크롤러 대응하기(SEO)

Google, Bing, Facebook, Twitter와 같은 소셜 미디어 사이트는 웹 애플리케이션 컨텐츠를 수집하고 검색에 활용하기 위해 웹 크롤러를 사용한다. 그런데 이런 웹 크롤러는 진짜 사람이 하는 것처럼 애플리케이션 페이지를 효율적으로 이동하면서 원하는 내용을 수집하지는 못한다. 대부분의 웹 크롤러, 봇들이 자바스크립트 파일을 실행시키지 못한다. 때문에 HTML 에서만 콘텐츠를 수집하게 되고 클라이언트 사이드 렌더링되는 페이지를 빈 페이지로 인식하게 된다. 웹 크롤러에 대응하는 과정은 [검색 엔진 최적화(search engine optimization, SEO)](https://static.googleusercontent.com/media/www.google.com/en//webmasters/docs/search-engine-optimization-starter-guide.pdf)라고도 합니다.



### 모바일 장비와 저사양 장비에서 동작하는 성능 끌어올리기

JavaScript를 지원하지 않는 디바이스가 존재하기도 하고 JavaScript를 실행하는 것이 오히려 사용자의 UX를 해치는 디바이스도 존재한다. 이런 경우에는 클라이언트에서 JavaScript를 실행하지 않고 서버에서 미리 렌더링된 앱을 보내서 간단하게 실행하는 것이 더 좋다. 앱을 이렇게 제공하면 원래 사용자에게 제공하려던 기능을 모두 제공할 수는 없겠지만, 앱을 전혀 사용할 수 없는 상황은 피할 수 있다.



### 첫 페이지를 빠르게 표시하기

사용자의 재방문을 유도하려면 첫 페이지를 빠르게 표시하는 것이 무엇보다 중요하다. 첫 페이지가 3초 안에 표시되지 않는다면 [53%의 모바일 사용자가 재방문하지 않는다는 통계](https://www.thinkwithgoogle.com/marketing-resources/data-measurement/mobile-page-speed-new-industry-benchmarks/)도 있다. 사이트를 방문한 사용자가 다른 곳으로 발길을 돌리는 것을 원하지 않는다면 앱을 최대한 빠르게 실행하는 것이 좋다.



> 참고 및 출처
>
> [서버 사이드 렌더링 (Server-side Rendering, SSR): Angular Universal](https://angular.kr/guide/universal)
>
> [Rendering on the Web](https://developers.google.com/web/updates/2019/02/rendering-on-the-web)
>
> [서버 사이드 렌더링(SSR:Server Side Rendering)이란?](https://linked2ev.github.io/devlog/2018/11/15/Javascript-2.-What-is-SSR/)
>
> [Single Page Application](https://poiemaweb.com/js-spa)