---
title: "Mobile Touch Swipe Indicator(20200625)"
date: 2020-06-25 16:00:00
category: 'Today I Learned'
draft: false
---

## ISSUE

- 캐러셀 UI에서 20개의 item을 보여야함

- 추가적인 item은 오른쪽 스와이핑 액션을 통해 해당 페이지로 넘어가야함

  <img width="600" alt="스크린샷 2020-06-27 오후 3 57 51" src="https://user-images.githubusercontent.com/36187948/85916788-f8f69d80-b88e-11ea-92a9-85bde0edb959.png">

- touchmove(스와이핑)이벤트가 발생해는 x값에 따라 퍼센테이지를 UI로 제공

<img width="600" alt="스크린샷 2020-06-27 오후 4 01 50" src="https://user-images.githubusercontent.com/36187948/85916862-94880e00-b88f-11ea-9951-fb4734941e4a.png">

<img width="600" alt="스크린샷 2020-06-27 오후 4 02 01" src="https://user-images.githubusercontent.com/36187948/85916863-9782fe80-b88f-11ea-87bc-b76b043ef0b2.png">

<img width="600" alt="스크린샷 2020-06-27 오후 4 02 11" src="https://user-images.githubusercontent.com/36187948/85916864-9782fe80-b88f-11ea-8839-8b41a3c16bf9.png">

## 해결 방안

- item의 개수가 20개 이상일 때에만 swipe indicator를 보여주기 위한 filtering을 한다.

```typescript
ngAfterViewInit() {
    this._sub.push(
      this.result$.pipe(
        tap(result => this.result = result),
        filter((result) => result && result.total > result.listNum),
        switchMap(() => this.swipeToSeeMore())
      ).subscribe()
    );
  }
```



- `touchmove` event를 감지해서 스크롤이 끝에 도달할 경우 이벤트를 전파한다. 
- 끝에 도달할 경우, touchStartX시점에서 touchmove의 이동량을 계산하여 xDiff(스크롤 이동량)을 계산한다.
- 디바이스 화면의 40%만큼 스크롤된 값 즉, xThreshold을 저장한다.(40%의 수치를 100%로 환산) => 40%이상 스크롤될 때 원하는 다음 동작을 실행
- 스크롤된 값에서 xThreshold값을 나누어 percentage값을 저장
- 40%이상의 스크롤 이후 `touchend` 이벤트가 발생하면 원하는 주소로 이동한다.
- 40%이하의 스크롤 이후 `touchend`이벤트가 발생하면 `translateX`값 및 `strokeDasharray` 값을 초기화한다.

```typescript
private swipeToSeeMore() {
    const element = this.elementRef.nativeElement;
    const circle = this.circle.nativeElement;
    let reqId, percentage, touchStartX;

    return combineLatest([
      fromEvent(element, 'touchmove', { passive: true }).pipe(
        filter(() => {
          const { scrollWidth, scrollLeft, clientWidth } = element;
          return scrollWidth - scrollLeft - clientWidth <= 0;
        }),
        map((touchMove: TouchEvent) => touchMove.changedTouches[0].clientX),
        tap((x) => touchStartX = touchStartX || x),
        tap((x) => {
          // 디바이스 가로폭 40%까지만 swipe 하면 100%
          const xThreshold = element.clientWidth * 0.4;
          const xDiff = touchStartX - x;
          percentage = Math.min(xDiff, xThreshold) / xThreshold;
          reqId = cancelAndRequestAnimationFrame(reqId, () => {
						// transform을 이용해 안드로이드 디바이스에서도 스와이핑이 되게 구현
            element.style.transform = `translateX(-${xDiff / 2}px)`;
            circle.style.strokeDasharray = `${Math.round(percentage * 100)}, 100`;
          });
        })
      ),
      fromEvent(element, 'touchend', { passive: true }).pipe(
        tap(() => {
          if (percentage >= 1) {
            this.router.navigate(this.navigateUrl);
          } else {
            reqId = cancelAndRequestAnimationFrame(reqId, () => {
              element.style.transform = `translateX(0px)`;
              circle.style.strokeDasharray = `0, 100`;
            });
          }
          touchStartX = 0;
        })
      )
    ]);
  };
```

이 밖에도 `strokeDasharray`, `requestAnimationFrame` 등 이 기능을 구현 및 최적화하기 위해 알아야할 지식들이 있다. 참고할 문서들이 많으니 링크로 남겨 놓겠다.

#### 결과 화면

<video width="600" controls>
  <source src="./sources/swipe.mov" type="video/mp4">
</video>




> ### 참고 및 출처
>
> - [Detect a finger swipe through JavaScript on the iPhone and Android](https://stackoverflow.com/questions/2264072/detect-a-finger-swipe-through-javascript-on-the-iphone-and-android)
>
> - [stroke-dasharray](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/stroke-dasharray)
>
> - [How to code a responsive circular percentage chart with SVG and CSS.](https://medium.com/@pppped/how-to-code-a-responsive-circular-percentage-chart-with-svg-and-css-3632f8cd7705)
>
> - [window.requestAnimationFrame()](https://developer.mozilla.org/ko/docs/Web/API/Window/requestAnimationFrame)

