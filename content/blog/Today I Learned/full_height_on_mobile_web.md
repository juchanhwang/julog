---
title: 'Full Height On mobile Web'
date: 2020-06-24 16:00:00
category: 'Today I Learned'
draft: false
---

## 문제의 상황 및 요구사항

전체 화면 높이를 차지하도록 요소의 스타일을 지정하려면 높이를 100vh로 설정하면된다. 하지만 예상되로 되지 않는 것을 경험할 것이다. 웹 브라우저에 기본적으로 보이는 주소 줄까지 높이로 계산해, 가려진 영역 때문에 스크롤이 발생한다.

<img width="900" alt="스크린샷 2020-06-24 오후 5 04 14" src="https://user-images.githubusercontent.com/36187948/85519623-be95c200-b63c-11ea-97a6-827ee866a9d7.png">



## 해결 방안

#### 사용자 정의 vh 사용 (CSS 변수)

window.innerHeight를 사용하여 CSS 변수 --vh를 설정한다.

```ts
// 1)
window.addEventListener('resize', () => { 
  document.querySelector(':root').style
    .setProperty('--vh', window.innerHeight/100 + 'px');
})

// 2)
const vh = window.innerHeight * 0.01;
this.elementRef.nativeElement.style.setProperty('--vh', `${vh}px`);
```

이제 전 영역에서 --vh를 사용할 수 있다. 

```scss
min-height: calc(100vh - #{$edge-height});
min-height: calc(var(--vh, 1vh) * 100 - #{$edge-height});
```

- 100vh - `height: calc(100 * var(--vh));`
- 10vh - `height: calc(10 * var(--vh));`
- 1vh - `height: calc(1 * var(--vh));`

> 참고 및 참조
>
> - [Mobile issue with 100vh | height: 100% !== 100vh [3 solutions]](https://dev.to/admitkard/mobile-issue-with-100vh-height-100-100vh-3-solutions-3nae)