---
title: 'position: sticky 값 주기'
date: 2020-04-08 16:00:00
category: 'Today I Learned'
draft: false
---



sticky가 적용된 엘리먼트의 부모 요소의 height 범위 내에서 fixed으로 작동되지만 그 외의 영여에서 static으로 동작한다.

그렇기 때문에 header나 footer영역에 sticky값을 적용하고 싶을 때, 최상위 엘리먼트를 부모로를 기준으로 sticky bar를 생성한다.

- 사용 예시

```html
<app-root>
	<header class="sticky"></header>
	<body></body>
</app-root>
```

```scss
.sticky {
	position: sticky;
	top: 0;
	display: block;
}
```

- 사파리 브라우저 호환

```scss
position: -webkit-sticky; 
position: sticky;
```

> 참고

- [CSS Position Sticky - How It Really Works!](https://medium.com/@elad/css-position-sticky-how-it-really-works-54cd01dc2d46)
- [CSS Position Sticky](https://medium.com/guleum/css-position-sticky-6371fb3d2939)

