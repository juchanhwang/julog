---
title: 'Sticky Header'
date: 2019-12-11 16:00:00
category: 'Today I Learned'
draft: false
---



#### sticky header

1. 보통의 경우

   ```scss
   header {
   	position: fixed;
   	top: 0
   	width: 100%
   }
   
   body {
   	padding-top: (header-height)px;
   }
   ```

   ```html
   <header>
   </header>
   <body>
   </body>
   ```

2. in Angular

   ```scss
   :host {
   	padding-top: (header-height)px;
   }
   
   header{
   	position: fixed;
   	top: 0
   	width: 100%
   }
   ```

   ```html
   <mapia-header>
   	<header>
   	</header>
   </mapia-header>
   <body>
   </body>
   ```
