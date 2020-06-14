---
title: 'image 안쪽의 border 값 주기 : MASK(20191226)'
date: 2019-12-26 16:00:00
category: 'Today I Learned'
draft: false
---



```html
<div class="image-container">
	<img>
</div>
```


```scss
.image-container {
	position: relative;
	
	&:before {
		content: '';
    display: block;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    border: 1px solid #000;
    opacity: 0.1;
	}
}
```

