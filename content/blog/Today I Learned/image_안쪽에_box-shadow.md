---
title: "image 안쪽에 box-shadow"
date: 2020-06-05 16:00:00
category: 'Today I Learned'
draft: false
---

## 문제의 상황 및 요구사항

- 이미지 자체 흰색 배경때문에 이미지의 경계가 불분명함
- 유저에게 어색한 UI 제공

<img width="270" alt="스크린샷 2020-06-06 오후 7 37 05" src="https://user-images.githubusercontent.com/36187948/83942302-41411380-a82d-11ea-9a4e-903eab03b6b4.png">

## 해결 방안

- 가상 요소 `:before`

```scss
img {
  &:before {
	  content: "";
    display: block;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    border: solid 1px rgba(0, 0, 0, 0.1);
    border-radius: 4px;
    z-index: 1;
	}
}
```

- box-shadow

```scss
img {
	-webkit-box-shadow: inset 0 0 0 1px rgba(0, 0, 0, 0.1);
	-moz-box-shadow: inset 0 0 0 1px rgba(0, 0, 0, 0.1);
	box-shadow: inset 0 0 0 1px rgba(0, 0, 0, 0.1);
}
```

=> border보다는 box-shadow를 줬다. border는 영역을 차지하고 box-shadow는 영역을 차지하지 않는다.

=> 경계를 줌으로써 조금은 더 자연스로운 UI를 보일 수 있다.

<img width="270" alt="스크린샷 2020-06-06 오후 7 37 13" src="https://user-images.githubusercontent.com/36187948/83942303-42724080-a82d-11ea-8fe5-7a26219d46d1.png">

