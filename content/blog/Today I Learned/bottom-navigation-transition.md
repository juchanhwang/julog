---
title: 'Bottom Navigation Scrolling(20191212)'
date: 2019-12-12 16:00:00
category: 'Today I Learned'
draft: true

---



```ts
import { Directive, ElementRef, Input, NgZone, Renderer2 } from '@angular/core';
import { GlobalState, selectCurrentURL } from '@mapiacompany/mapia-core';
import { Store } from '@ngrx/store';
import { AbstractBaseComponent, enablePassive } from '@mapiacompany/armory';

@Directive({ selector: '[footerScroller]' })
export class MobileFooterScroller extends AbstractBaseComponent {
  @Input() footerHeight: number;

  beforePageYOffset = 0;
  currentUrl$ = this.store$.select(selectCurrentURL);
  reqAniId: number;

  prevState: string;

  constructor(
    public elementRef: ElementRef,
    public renderer: Renderer2,
    public zone: NgZone,
    public store$: Store<GlobalState>
  ) {
    super();
  }

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



```html
<div footerScroller footerHeight="45">
  <div class="footer-nav">
    <a routerLink="/main" routerLinkActive="active">
      <i class="fal fa-home"></i>
      <div class="title">{{'home' | translateExt}}</div>
    </a>
    <a routerLink="/piano/sheet" routerLinkActive="active" #rlaMusic="routerLinkActive" class="sheet">
      <i class="fal fa-music"></i>
      <div class="title">{{'sheet' | translateExt}}</div>
    </a>
    <a routerLink="/video" routerLinkActive="active" #rlaYoutube="routerLinkActive">
      <i class="fab fa-youtube"></i>
      <div class="title">{{'video' | translateExt}}</div>
    </a>
    <a routerLink="/community" routerLinkActive="active" #rlaFree="routerLinkActive">
      <i class="fal fa-users"></i>
      <div class="title">{{'community' | translateExt}}</div>
    </a>
    <a routerLink="/store/main" [class.active]="isStore">
      <i class="fal fa-shopping-cart"></i>
      <div class="title">{{'store' | translateExt}}</div>
    </a>
  </div>
</div>
```

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

