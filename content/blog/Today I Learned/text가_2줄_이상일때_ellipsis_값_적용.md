---
title: 'text가 2줄 이상일때 ellipsis 값 적용'
date: 2020-03-30 16:00:00
category: 'Today I Learned'
draft: false
---



```scss
> .tile-title {
    height: 44px;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
  }
```

`-webkit-line-clamp: 2;` 원하는 줄에 ellipsis를 적용할 수 있음. (단 IE는 지원하지 않는다. height값 적용해야함)

