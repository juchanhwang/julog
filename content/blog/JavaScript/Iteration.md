---
title: 'Iteration'
date: 2020-08-20 16:00:00
category: 'JavaScript'
draft: true
---



# 이터레이션 프로토콜

ES6에서 도입된 이터레이션 프로토콜(iteration protocol)은 데이터 컬렉션을 순회하기 위한 프로토콜(미리 약속된 규칙)이다. **이터레이션 프로토콜을 준수한 객체는 for…of 문으로 순회할 수 있고 [Spread 문법](https://poiemaweb.com/es6-extended-parameter-handling#3-spread-문법)의 피연산자가 될 수 있다.**

이터레이션 프로토콜에는 이터러블 프로토콜(iterable protocol)과 이터레이터 프로토콜(iterator protocol)이 있다.

<img width="880" alt="스크린샷 2020-08-20 오후 10 03 04" src="https://user-images.githubusercontent.com/36187948/90773513-fa918e80-e330-11ea-8ae6-1862d0e314be.png">

## Iterable(이터러블)

이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 이터러블은 **Symbol.iterator 메소드**를 구현하거나 프로토타입 체인에 의해 상속한 객체를 말한다. Symbol.iterator 메소드는 이터레이터를 반환한다. 이터러블은 for…of 문에서 순회할 수 있으며 Spread 문법의 대상으로 사용할 수 있다.

배열은 Symbol.iterator 메소드를 소유한다. 따라서 배열은 이터러블 프로토콜을 준수한 이터러블이다.

```

```



## Iterator(이터레이터)