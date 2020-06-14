---
title: '새로운 translateExt 번역 package가 생겼다!(20191107)'
date: 2019-11-07 16:00:00
category: 'Today I Learned'
draft: false
---



#### 사용 방법

- ts code에 providers에 이렇게 추가해주면 된다.

  ```typescript
    @Component({
      selector: 'mp-paper-upload',
      templateUrl: './paper-upload.component.html',
      styleUrls: [ './paper-upload.component.scss' ],
      providers: [ setTranslatePrefix('mp-paper-upload') ]
    })
  ```

- template file에 원하는 translateExt pipe를 통해 원하는 번역 데이터를 불러온다

  ```html
  <h2>{{ 'paper-upload' | translateExt }}</h2>
  ```



#### 기존의 translate과의 비교

```html
<h2>{{ '**kokomu-paper-upload.**paper-upload' | translateExt }}</h2>

<h2>{{ 'paper-upload' | translateExt }}</h2>
```

⇒ prefix의 중복이 없어졌다!! tscode에서 prefix 부분만 수정해주면 template의 모든 코드를 수정할 필요없이 바로 사용할 수 있는 이점이 생겼다
