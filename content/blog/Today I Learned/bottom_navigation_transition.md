---
title: 'Bottom Navigation Scrolling'
date: 2020-07-06 16:00:00
category: 'Today I Learned'
draft: false
---

### 

### 

## TODO

- 아래로 스크롤할 때, 공간의 활용을 위해 네비게이션을 아래로 숨겨야함
-  위로 스크롤을할 때 다시 네이베이션을 보여야함
- 스크롤이 bottom에 닿으면 네비게이션을 보여야함

<video width="100%" height="400" controls>
  <source src="./sources/bottom_navigation_scrolling.mov" type="video/mp4">
</video>

### 

### 

> TS

```ts
@Directive({ selector: '[footerScroller]' })
export class MobileFooterScroller {
  @Input() footerHeight: number;

  beforePageYOffset = 0;
  currentUrl$ = this.store$.select(selectCurrentURL);
  reqAniId: number;

  prevState: string;

  ngAfterViewInit(): void {
    this._sub.push(
      this.currentUrl$.subscribe(
        () => {
          this.addClass('sticky');
          this.removeClass('hide-footer');
        }
      )
    );

    this.zone.runOutsideAngular(() => {
      window.addEventListener(
        'scroll',
        this.onWindowScroll,
        enablePassive());
    });
  }

  ngOnDestroy() {
    this.zone.runOutsideAngular(() => {
      window.removeEventListener('scroll', this.onWindowScroll);
    });
  }

  onWindowScroll = () => {
    const yOffset = window.pageYOffset;
    const currentState = this.getCurrentState(yOffset);
    if (this.prevState !== currentState) {
      if (this.reqAniId) {
        cancelAnimationFrame(this.reqAniId);
      }

      this.reqAniId = requestAnimationFrame(() => {
        switch (currentState) {
          case 'bottom':
            this.removeClass('hide-footer');
            this.addClass('sticky');
            break;

          case 'up':
            this.removeClass('hide-footer');
            this.addClass('sticky');
            break;

          case 'down':
            this.removeClass('sticky');
            this.addClass('hide-footer');
            break;
        }
      });
    }

    this.prevState = currentState;
    this.beforePageYOffset = yOffset;
  };

  getCurrentState(yOffset) {
    const IS_SCROLL_BOTTOM = (window.innerHeight + window.pageYOffset) >= document.body.offsetHeight - this.footerHeight;
    const IS_SCROLL_UP = this.beforePageYOffset > yOffset && yOffset > this.footerHeight;
    const IS_SCROLL_DOWN = yOffset > this.footerHeight;

    let currentState: string;
    if (IS_SCROLL_BOTTOM) {
      currentState = 'bottom';
    } else if (IS_SCROLL_UP) {
      currentState = 'up';
    } else if (IS_SCROLL_DOWN) {
      currentState = 'down';
    }

    return currentState;
  }

  addClass(className: string) {
    this.renderer.addClass(this.elementRef.nativeElement, className);
  }

  removeClass(className: string) {
    this.renderer.removeClass(this.elementRef.nativeElement, className);
  }
}
```

### 

> HTML

```html
<div footerScroller footerHeight="45">
  <div class="footer-nav">
    <a routerLink="/main" routerLinkActive="active"></a>
    <a routerLink="/piano/sheet" routerLinkActive="active"></a>
    <a routerLink="/video" routerLinkActive="active"></a>
    <a routerLink="/community" routerLinkActive="active"></a>
    <a routerLink="/store/main" routerLinkActive="active"></a>
  </div>
</div>
```

### 

> SCSS

- 부드러운 ux를 위해 transition을 이용한다.

```scss
:host > div[footerScroller] {
  > div {
    position: relative;
    -webkit-transition: bottom 0.35s cubic-bezier(0.165, 0.84, 0.44, 1);
    -moz-transition: bottom 0.35s cubic-bezier(0.165, 0.84, 0.44, 1);
    -o-transition: bottom 0.35s cubic-bezier(0.165, 0.84, 0.44, 1);
    transition: bottom 0.35s cubic-bezier(0.165, 0.84, 0.44, 1);
  }

  &.sticky {
    > div {
      bottom: 0;
    }
  }

  &.hide-footer {
    > div {
      bottom: -$tab-menu-height;
      bottom: calc((constant(safe-area-inset-bottom) + #{$tab-menu-height}) * -1) !important;
      bottom: calc((env(safe-area-inset-bottom) + #{$tab-menu-height}) * -1) !important;
    }
  }
}
```

