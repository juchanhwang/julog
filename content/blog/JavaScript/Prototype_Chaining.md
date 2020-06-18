---
title: 'Prototype Chaining'
date: 2020-06-15 16:00:00
category: 'JavaScript'
draft: true
---



배열 리터럴은  Array생성자 함수와 그 프로토타입으로 이루어져 있다.

<img width="600" alt="스크린샷 2020-06-15 오후 11 44 34" src="https://user-images.githubusercontent.com/36187948/84671356-3d666d00-af62-11ea-87ce-14ca7da24fea.png">



Object.prototype에는 자바스크립트 전체를 통괄하는 공통된 메소드들이 있다.

- hasOwnProperty()
- toString()
- valueOf()
- isPrototypeOf()

이 메소드들은 모든 데이터타입이 프로토타입 체이닝을 통해서 접근할 수 있다.