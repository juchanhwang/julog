---
title: 'Sticky Footer'
date: 2019-12-10 16:00:00
category: 'Today I Learned'
draft: false
---



#### sticky footer bottom (footer 하단에 고정)

```scss
.container {
	min-height: 100%;
	display: flex;
	flex-direction: column;

	.footer {
		margin-top: auto;
	}
}
```

- min-height: 100%

  - body > contaier > footer인 경우
  - container가 body height 값을 갖고있지 않을 경우가 있음(body보다 작을 경우)

- `margin-top: auto` 속성으로 자식 요소를 화면 아래에 배치

  flex item에 `margin-top: auto` 속성을 적용하면 바깥 여백이 flex item을 위쪽에서 아래쪽으로 밀기 때문에 flex item이 아래쪽에 위치하게 된다.

> - [flexbox로 만들 수 있는 10가지 레이아웃](https://d2.naver.com/helloworld/8540176)

- [css로 footer를 하단에 고정하는 5가지 방법](https://m.blog.naver.com/PostView.nhn?blogId=eggtory&logNo=220744380205&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

