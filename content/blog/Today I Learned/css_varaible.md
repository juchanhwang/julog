---
title: "css variable"
date: 2020-07-19 16:00:00
category: 'Today I Learned'
draft: false
---

## css variable 사용하기

> 최상단 css파일 루트에 선언

```scss
:root {
  --primary0: #fdf6f6;
  --primary10: #fbe7e6;
  --primary20: #f3b1ad;
  --primary30: #ef928c;
  --primary40: #e8625a;
  --primary50: #d05851;
  --primary60: #ba4e48;
  --primary70: #a2453f;
  --primary80: #8c3b36;
  --primary90: #803d39;

  --secondary0: #ebebff;
  --secondary10: #c1c1ff;
  --secondary20: #8282ff;
  --secondary30: #5858ff;
  --secondary40: #2f2fff;
  --secondary50: #2a2ae5;
  --secondary60: #2525cc;
  --secondary70: #2020b2;
  --secondary80: #1c1c99;
  --secondary90: #17177f;
  ...
}
```

<br>

## 사용법

> 경로를 import할 필요없이 변수를 바로 사용할 수 있다.

```scss
// import "color" => import할 필요x

div {
 color: var(--primary10); 
}
```

