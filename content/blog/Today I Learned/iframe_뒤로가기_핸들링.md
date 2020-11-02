---
title: "Iframe 뒤로가기 핸들링"
date: 2020-11-02 16:00:00
category: 'Today I Learned'
draft: true
---

```ts
export class IframeUIController implements UIControllerInterface {
  onDestroy$ = new Subject<PayPortCustomError>(); // do nothing (WindowPopupController 때문에 넣음)
  popstateEvent: Subscription | undefined;
  iframe: HTMLIFrameElement;

  constructor(
    private iframeContainer: HTMLElement
  ) {
    this._createIframe();

    if (IS_MOBILE()) {
      history.pushState(null, '', location.href);

      this.popstateEvent = fromEvent(window, 'popstate').pipe(
        tap(() => this.onDestroy$.error(new PayPortCustomError('결제가 취소되었습니다.')))
      ).subscribe();
    }
  }

  setUrl(url: string) {
    if (!this.iframe) throw new Error(`iframe ${DEFAULT_PAYPORT_IFRAME_NAME}이 존재하지 않습니다.`);
    this.iframe.src = url;
  };

  destroy() {
    if (this.iframe) {
      this.iframe.remove();
      this.popstateEvent?.unsubscribe();
      // TODO: route 시스템 문제시 pushState 했던 것 popState
    }
  };

  private _createIframe() {
    this.iframe = document.createElement('iframe');
    this.iframe.setAttribute('id', DEFAULT_PAYPORT_IFRAME_NAME);
    this.iframe.setAttribute('name', DEFAULT_PAYPORT_IFRAME_NAME);
    this.iframe.style.height = '100%';
    this.iframe.style.width = '100%';
    this.iframeContainer.appendChild(this.iframe);
  }
}
```