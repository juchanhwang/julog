---
title: 'switch(20191227)'
date: 2019-12-27 16:00:00
category: 'Today I Learned'
draft: false
---



- switch

  `switch(expression)`

  expressiont 값과 case의 값이 일치하는지 평가하는 문법이다.

  - special label block
    - 중괄호('{}"): 언어 파서가 해석하기 위한 토큰 문법적인 요소이다.
  - Fall Through

  EX) changeable responsibility(디자인 패턴)

  ```tsx
  switch(true) {
  	case network() === 'online':
  	case network() === 'wifi':
  	case network() === 'offline':
  	case localCache():
  	default: // 안내문..
  }
  ```

  > [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/switch) [코드스피츠](https://youtu.be/FP9LBhPD4eo?list=PLBNdLLaRx_rIF3jAbhliedtfixePs5g2q&t=2136)
