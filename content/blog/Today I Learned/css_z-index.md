---
title: "CSS z-index"
date: 2020-06-17 16:00:00
category: 'Today I Learned'
draft: false
---

분명하진 않지만 HTML 요소는 3차원으로 생성된다. x축과 y축에 정렬된 것 외에도 z축에 놓일 수 있고, 3차원에서의 위치를 제어한다.

![스크린샷 2020-06-17 오전 9 34 26](https://user-images.githubusercontent.com/36187948/84841616-cde5a000-b07d-11ea-8893-21bdf8c0eca5.png)

z-index는 relative, absolute 또는 fixed와 같은 position 값이 있을 때에만 적용된다.

## Stacking Level

스태킹 레벨은 현재 요소가있는 z 축의 값이다. 숫자가 클수록 요소가 요소 스택에서 높고 화면 표면에 더 가깝다는 것을 의미한다.

![스크린샷 2020-06-17 오전 9 42 27](https://user-images.githubusercontent.com/36187948/84841958-dd191d80-b07e-11ea-98a5-589c6eada4ae.png)

만약 모든 엘리먼트들에 z-index가 지정되지 않았을 경우에는 엘리먼트들이 다음 순서로 아래에서부터 위로 쌓인다.

1.  뿌리 엘리먼트의 배경과 테두리
2. 자식 엘리먼트들은 HTML에서 등장하는 순서대로
3. position이 지정된 자식 엘리먼트들은 HTML에서 등장하는 순서대로

![스크린샷 2020-06-17 오전 9 45 41](https://user-images.githubusercontent.com/36187948/84842128-50229400-b07f-11ea-9d70-3d437d3c54a7.png)

- position 속성이 지정되지 않은 블록(DIV #5)은 항상 position이 지정된 엘리먼트 이전에 렌더링 된다. 따라서 position이 지정된 엘리먼트 아래에 보인다. 설령 HTML 문서상에서 먼저 나오더라도 position이 지정되지 않은 엘리먼트는 지정된 엘리먼트보다 아래에 보인다. 

## Stacking Context(쌓임 맥락)

**쌓임 맥락**(stacking context)은 가상의 Z축을 사용한 HTML 요소의 3차원 개념화이다. Z축은 사용자 기준이며, 사용자는 뷰포트 혹은 웹페이지를 바라보고 있을 것으로 가정한다. 각각의 HTML 요소는 자신의 속성에 따른 우선순위를 사용해 3차원 공간을 차지한다.

자식의 `z-index` 값은 부모에게만 의미가 있다. 하나의 쌓임 맥락은 부모 쌓임 맥락 안에서 통째로 하나의 단위로 간주된다.

![스크린샷 2020-06-17 오전 9 50 55](https://user-images.githubusercontent.com/36187948/84842424-0ab29680-b080-11ea-961f-67544b4f8471.png)

> ### 출처 및 참고
>
> - [CSS z-index 이해하기(MDN)](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_Positioning/Understanding_z_index)
>
> - [How z-index Works](https://bitsofco.de/how-z-index-works/)