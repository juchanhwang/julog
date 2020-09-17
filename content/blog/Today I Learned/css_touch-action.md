---
title: 'css touch-action'
date: 2020-09-17 16:00:00
category: 'Today I Learned'
draft: false
---

## [touch-action](https://developer.mozilla.org/en-US/docs/Web/CSS/touch-action): 브라우저에게 맡길 터치 액션 지정

기본적으로 터치 이벤트의 처리는 브라우저가 담당하는 영역이다. `touch-action` 속성을 통해 어떤 요소 내에서 브라우저가 처리할 터치 액션의 목록을 지정할 수 있다. 표준 터치 제스쳐로는 터치를 사용한 스크롤(panning)과 여러 손가락을 사용한 확대/축소(pinch zoom)이 존재하며, 브라우저에 따라 더블 탭으로 확대 등 표준이 아닌 여러 제스쳐를 지원하는 경우도 있다.

`touch-action` 속성의 값으로 `auto` 이외의 값을 줄 경우, 해당 속성에 명시해준 터치 액션만이 브라우저에 의해 처리된다. 예를 들어, `touch-action: pinch-zoom` 속성을 갖는 엘리먼트에서는 터치를 사용한 스크롤이 (자바스크립트로 별도로 처리를 해 주지 않는 이상) 무시된다.

### 가능한 속성 값

```css
touch-action: auto; /* 기본 값 */
touch-action: none; /* 브라우저가 모든 터치 이벤트를 무시하도록 설정 */

touch-action: pan-x; /* 특정 축으로의 터치를 사용한 스크롤 허용 */
touch-action: pan-y;

touch-action: pan-left; /* 특정 방향으로의 터치를 사용한 스크롤 허용 */
touch-action: pan-right;
touch-action: pan-up;
touch-action: pan-down;

touch-action: pinch-zoom; /* 핀치 줌(여러 손가락을 사용한 확대/축소) 허용 */

touch-action: manipulation; /* 터치를 사용한 스크롤, 핀치 줌만 허용하고 그 외 비표준 동작 (더블 탭으로 확대 등) 불허용 */

touch-action: pan-y pinch-zoom; /* 동시에 여러 값 지정 가능 */
```

<br>

#### 브라우저 호환성

<img width="100%" height="300px" alt="touch-action" src="https://user-images.githubusercontent.com/36187948/93486980-c6b18500-f93f-11ea-812d-906cb07b9e7f.png">

<br>

> ### 참고 및 참조
>
> - [잘 알려지지 않은 유용한 CSS 속성들](https://ahnheejong.name/articles/less-famous-css-properties/)
>
> - [touch-action [MDN]](https://developer.mozilla.org/en-US/docs/Web/CSS/touch-action)