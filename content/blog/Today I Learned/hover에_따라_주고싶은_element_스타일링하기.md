---
title: "hover에 따라 주고싶은 element 스타일링하기(20200510)"
date: 2020-05-10 16:00:00
category: 'Today I Learned'
draft: false
---



ex1)  인접한 경우

```html
<div class="a">
  <div>hover me</div>
</div>
<div class="b">
  <div>style me</div>
</div>
```

```scss
.a:hover + .b > div {
    background: #ccc
}
```

Ex2) 인접하지 않은 경우

```html
<div class="a">
  <div>hover me</div>
</div>
<div>hello</div>
<div>world</div>
<div class="b">
  <div>style me</div>
</div>
```

```scss
.a:hover ~ .b > div {
    background: #ccc
}
```

Ex3) parent to child

```html
<div class="parent">
  hover me
  <div class="child">style me</div>
</div>
```

```scss
.parent:hover .child {
  color: yellow
}
```

Ex3) child to parent

```html
<div class="parent">
  style me
  <div class="child">hover me</div>
</div>
```

```scss
div.parent {  
    pointer-events: none;
}

div.child {
    pointer-events: auto;
}

div.parent:hover {
    background: yellow;
}
```
