---
title: "[Angular] popstate이벤트를 통해 iframe의 뒤로가기 제어하기"
date: 2020-05-29 16:00:00
category: 'Today I Learned'
draft: false
---



## 문제 상황

1. 한 컴포넌트 안에서 mobiliance 결제 iframe을 랜더해야하는 상황이다.

   \#1

   <img width="250" alt="스크린샷 2020-05-29 오후 7 09 45" src="https://user-images.githubusercontent.com/36187948/83248604-488a7080-a1e0-11ea-9f06-68bae1c56895.png">

   \#2

   <img width="250" alt="스크린샷 2020-05-29 오후 7 13 51" src="https://user-images.githubusercontent.com/36187948/83248824-9dc68200-a1e0-11ea-8ff4-7f54e7ae3c53.png">

2. \#2에서 뒤로가기를 했을 때 #1이 나와야한다.

3. 하지만 당연하게도 그 전 페이지로 가진다. #1, #2 모두 하나의 컴포넌트(페이지)이기 때문이다.

4. popstate이벤트로 뒤로가기를 제어 했을 때 iframe에서 뒤로가기 이벤트가 trigger되지 않는다.

5. iframe이 현재의 컴포넌트가(#1) 아니라 iframe에서 자체적으로 뒤로가기가(틀릴 수도 있다ㅠ) 되어 'blank', 비어있는 흰 화면으로 이동한다. (뒤로가기를 한 번 더해야 popstate event가 발생)



## 해결 방안

- history.pushState 메소드와 popstate이벤트를 통해 뒤로가기를 제어한다.
- 매번 iframe을 동적으로 랜더한다. (뒤로가기 했을 때 기존 iframe 제거 및 결제 옵저버 구독 해지)
- iframe 랜더링 방식이 다른 kakao와 toss결제는 api호출을 통해 `message`이벤트를 발생시켜 iframe을 제거한다.

### 기존 iframe 랜더링: 정적인 방식

```html
<iframe [class.show]="isProcessing && (payInfo.pg == 'mob' || payInfo.pg == 'kakaopay' || payInfo.pg == 'toss')"
        name="phoneIframe" id="phoneIframe" width="100%"></iframe>
```

```scss
iframe[name=phoneIframe] {
  display: none;
  border: none;
  height: calc(100vh - #{$header-height + $my-cash-height + $tab-height + 5px});

  &.show {
    display: block;
  }
}
```

### 수정 후 iframe 랜더링: 동적인 방식

```html
<div class="iframe-container"
     [class.show]="isProcessing && (payInfo.pg == 'mob' || payInfo.pg == 'kakaopay' || payInfo.pg == 'toss')"></div>
```

```scss
.iframe-container {
  display: none;

  ::ng-deep iframe[name=phoneIframe] {
    border: none;
    height: calc(100vh - #{$header-height + $my-cash-height + $tab-height + 5px});
  }

  &.show {
    display: block;
  }
}
```

```tsx
export class MobileCashComponent {
  @HostListener('window:popstate')
  @HostListener('window:message')
  closePayment() {
    if (this.isProcessing) {
      this.closeMCASHPaymentResult();
    }
  }

	execute() {
	  this.createIframe();
	  super.execute();
	  history.pushState(null, null, location.href);
	}

	createIframe() {
	  const iframeContainer = document.querySelector('.iframe-container');
	  let iframe = iframeContainer.querySelector('#phoneIframe');
	  if (iframe) iframe.remove(); // 기존 태그 제거해버리기
	  iframe = document.createElement('iframe');
	  iframe.name = 'phoneIframe';
	  iframe.id = 'phoneIframe';
	  iframe.style.width = '100%';
	  iframeContainer.appendChild(iframe);

	  if (this.payInfo.pg === 'toss' || this.payInfo.pg === 'kakaopay') {
	    document.getElementById('phoneIframe').src = `${this.apiClient.base_api_url}/payment/mobilians/close`;
	  }
	}
}
```

> 참고 :
>
> - [popstate (MDN)](https://developer.mozilla.org/ko/docs/Web/API/Window/popstate_event)
>
> - [Iframe 이용시 HistoryBack event 문제 해결](https://devman.tistory.com/entry/Iframe-이용시-HistoryBack-event-문제-해결)
