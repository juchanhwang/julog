---
title: "Mobile Web Finger Swipe 제어하기(20200625)"
date: 2020-06-25 16:00:00
category: 'Today I Learned'
draft: true
---

## 문제의 상황 및 요구사항

```ts
combineLatest([
  fromEvent(element, 'touchmove', { passive: true }).pipe(
    filter(() => {
      const { scrollWidth, scrollLeft, clientWidth } = this.elementRef.nativeElement;
      return scrollWidth - scrollLeft - clientWidth <= 0;
    }),
    map((touchMove: TouchEvent) => touchMove.changedTouches[0].clientX),
    tap((x) => touchStartX = touchStartX || x),
    tap((x) => {
      const maxDiff = element.clientWidth * 0.3; // 디바이스 가로폭 30%까지만 스크롤 추가 허용
      const xDiff = touchStartX - x;
      percentage = Math.min(xDiff, maxDiff) / maxDiff;
      if (reqId) cancelAnimationFrame(reqId);
      reqId = requestAnimationFrame(() => {
        element.style.transform = `translateX(-${xDiff / 2}px)`;
        this.circle.nativeElement.style.strokeDasharray = `${Math.round(percentage * 100)}, 100`;
      });
    })
  ),
  fromEvent(element, 'touchend', { passive: true }).pipe(
    tap(() => {
      if (percentage >= 1) {
        this.router.navigate(this.navigateUrl);
      } else {
        if (reqId) cancelAnimationFrame(reqId);
        reqId = requestAnimationFrame(() => {
          element.style.transform = `translateX(0px)`;
          this.circle.nativeElement.style.strokeDasharray = `0, 100`;
        });
      }
      touchStartX = 0;
    })
  )
]).subscribe()
```