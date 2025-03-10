---
title: "[ANGULAR] how to prevent scroll"
date: 2020-05-27 16:00:00
category: 'Today I Learned'
draft: false
---



개발을 하다보면 스크롤을 잠깐 막아야하는 상황이 발생하곤 한다.

검색바를 예로들어보자. 첫 번째 요구사항은 검색창이 뜨면 backdrop 효과를 주는 것이고, 두 번째는 검색 input영역외의 바깥영역에 스크롤을 했을 때 스크롤을 주지 않는 것이다.

<img width="378" alt="스크린샷 2020-05-26 오전 12 52 49" src="https://user-images.githubusercontent.com/36187948/82828292-ad498080-9eeb-11ea-8cb4-745f09404bdc.png">

scroll event는 element layer에 상관없이 안쪽 영역의 레이어로 이벤트가 전파된다. 이벤트 버블링 현상인가..? 헷갈리지만 여튼 그렇다.

전파를 막기 위해서는 아래와 같은 방법들이 있다.

```ts
export function enableScroll() {
  document.querySelector('body').style.overflowY = 'auto';
}
export function disableScroll() {
  document.querySelector('body').style.overflowY = 'hidden';
}
```

⇒ PC에선 문제 없이 작동한다. 하지만 iOS 사파리랑 크롬에서는 무용지물이다.

```ts
export function enableScroll() {
  document.querySelector('body').style.position = 'relative';
  document.querySelector('body').style.height = '100%';
  document.querySelector('body').style.width = '100%';
}
export function disableScroll() {
  document.querySelector('body').style.position = 'fixed';
  document.querySelector('body').style.height = '100%';
  document.querySelector('body').style.width = '100%';
}
```

⇒ mobile keyboard height만큼 스크롤이 됨.

- 해결 방법

1. HotListener로 이벤트 등록

```ts
@HostListener('scroll', [ '$event' ])
@HostListener('touchmove', [ '$event' ])
@HostListener('mousewheel', [ '$event' ])
preventScroll(ev) {
  ev.preventDefault();
  ev.stopPropagation();
}
```

2. Rxjs fromEvent operator로 이벤트 등록

```ts
ngAfterViewInit() {
    const events = [ 'scroll', 'mousewheel', 'touchmove' ];
    this._sub.push(
      from(events).pipe(
        mergeMap((event) => fromEvent(this.elementRef.nativeElement, event))
      ).subscribe((event: Event) => {
          event.preventDefault();
          event.stopPropagation();
      })
    );
  }
```

이벤트를 등록하고 preventDefault 메소드와 stopPropagation 메소드를 사용해서 이벤트 전파를 막는 것이다.



#### 기타 상황에서의 스크롤 제어

- 모바일 디바이스에서 스크롤 제어

```html
<div (mousemove)="preventScroll($event)"</div>
```

```ts
preventScroll(ev) {
  ev.preventDefault();
  ev.stopPropagation();
}
```



- api call을 통해 동적으로 랜더된 list UI에서 스크롤 생성 및 제어

```ts
preventScroll(ev) {
  const isOverflow = this.elementRef.nativeElement.scrollHeight > this.elementRef.nativeElement.clientHeight;
  if (!isOverflow) {
    ev.preventDefault();
    ev.stopPropagation();
  }
}
```

```scss
container {
	max-height: calc(50vh - #{$header-search-input-height} * 2);
	overflow-y: scroll;
}
```

결과 화면

<img width="378" alt="스크린샷 2020-05-26 오전 12 52 49" src="https://user-images.githubusercontent.com/36187948/84358698-7ae59600-ac02-11ea-8ef3-ce8d6a6128b5.png">
