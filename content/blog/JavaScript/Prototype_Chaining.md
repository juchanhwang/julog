---
title: 'Prototype Chaining'
date: 2020-06-15 16:00:00
category: 'JavaScript'
draft: false
---



배열 리터럴은  Array생성자 함수와 그 프로토타입으로 이루어져 있다. Array.prototype에는 배열 메소드가 모두 담겨있다. Array의 prototype은 객체이다. `__proto__`가 생략 가능하기 때문에 배열 인스턴스가 마치 자신의 메소드인 것처럼 호출이 가능하다. 

<img width="600" alt="스크린샷 2020-06-15 오후 11 44 34" src="https://user-images.githubusercontent.com/36187948/84671356-3d666d00-af62-11ea-87ce-14ca7da24fea.png">

즉, **Object 생성자 함수의 new 연산자로 생성된 인스턴스**이다.  따라서  Object의 prototype을 상속 받는다. 배열은 Object.prototype에 있는 메소드를 마치 자신의 것처럼 사용할 수 있다. 이처럼 `__proto__` 라는 빨간 선을 따라서 연결된 것을 **프로토타입 체이닝**이라한다.

<img width="600" alt="스크린샷 2020-06-21 오후 3 17 11" src="https://user-images.githubusercontent.com/36187948/85218141-52178a80-b3d2-11ea-9add-a21bbda91e6d.png">

Object.prototype에는 자바스크립트 전체를 통괄하는 공통된 메소드들이 있다.

- hasOwnProperty()
- toString()
- valueOf()
- isPrototypeOf()

이 메소드들은 모든 데이터타입이 프로토타입 체이닝을 통해서 접근할 수 있다.