---
title: "css grid와 scroll snap 사용하기"
date: 2020-06-08 16:00:00
category: 'Today I Learned'
draft: false
---



## 문제의 상황 및 요구사항

- 세로로 나열되어있는 list를 grid 형태로 수정
- 가로형 스크롤로 UI 구현

<img width="300" alt="순위" src="https://user-images.githubusercontent.com/36187948/84028795-51dbc000-a9cc-11ea-9d45-10c8cfeabd48.png">





## 해결 방안

- css grid와 scroll snap을 이용하여 구현

```scss
.container {
  display: grid;
  overflow-x: scroll;
  scroll-snap-type: x mandatory;
  grid-auto-flow: column;
  grid-template-columns: 85% 100%;
  grid-template-rows: repeat(5, auto);
  grid-row-gap: 20px;
  grid-column-gap: 12px;
}
```

```scss
.item {
  display: flex;
  scroll-snap-align: center;
}
```

생각보다 어렵지 않은 작업이다. slick-carousel이나 js 코드로 구현할 필요 없이 css 코드 몇 줄이면 쉽게 구현할 수 있다.

- 결과 화면

<img width="350"  alt="순위" src="https://user-images.githubusercontent.com/36187948/84004692-e9c6b300-a9a6-11ea-9f29-0d6f6d010082.png"> <img width="350" alt="순위" src="https://user-images.githubusercontent.com/36187948/84004696-eaf7e000-a9a6-11ea-806f-535114ed0324.png">

> 참고 및 출처
> [scroll-snap-type(MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-snap-type)
