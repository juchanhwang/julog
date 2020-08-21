---
title: 'Iteration'
date: 2020-08-21 16:00:00
category: 'JavaScript'
draft: false
---



# 이터레이션 프로토콜

ES6에서 도입된 이터레이션 프로토콜(iteration protocol)은 데이터 컬렉션을 순회하기 위한 프로토콜(미리 약속된 규칙)이다. **이터레이션 프로토콜을 준수한 객체는 for…of 문으로 순회할 수 있고 [Spread 문법](https://poiemaweb.com/es6-extended-parameter-handling#3-spread-문법)의 피연산자가 될 수 있다.**

이터레이션 프로토콜에는 이터러블 프로토콜(iterable protocol)과 이터레이터 프로토콜(iterator protocol)이 있다.

<img width="880" alt="스크린샷 2020-08-20 오후 10 03 04" src="https://user-images.githubusercontent.com/36187948/90773513-fa918e80-e330-11ea-8ae6-1862d0e314be.png">

<br>

<br>

## Iterable(이터러블)

이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 이터러블은 **Symbol.iterator 메소드**를 구현하거나 프로토타입 체인에 의해 상속한 객체를 말한다. Symbol.iterator 메소드는 이터레이터를 반환한다. 이터러블은 for…of 문에서 순회할 수 있으며 Spread 문법의 대상으로 사용할 수 있다.

배열은 Symbol.iterator 메소드를 소유한다. 따라서 배열은 이터러블 프로토콜을 준수한 이터러블이다.

```js
const array = [1, 2, 3];

// 배열은 Symbol.iterator 메소드를 소유한다.
// 따라서 배열은 이터러블 프로토콜을 준수한 이터러블이다.
console.log(Symbol.iterator in array); // true

// 이터러블 프로토콜을 준수한 배열은 for...of 문에서 순회 가능하다.
for (const item of array) {
  console.log(item);
}
```

<br>

> Custom iterable

```js
var myIterable = {
    *[Symbol.iterator]() {
        yield 1;
        yield 2;
        yield 3;
    }
}

for (let value of myIterable) { 
    console.log(value); 
}
// 1
// 2
// 3
// or

[...myIterable]; // [1, 2, 3]
```

<br>

<br>

## Iterator(이터레이터, 반복자)

이터레이터 프로토콜은 next 메소드를 소유하며 next 메소드를 호출하면 이터러블을 순회하며 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 것이다. 이 규약을 준수한 객체가 이터레이터이다.

이터러블 프로토콜을 준수한 이터러블은 Symbol.iterator 메소드를 소유한다. 이 메소드를 호출하면 이터레이터를 반환한다. 이터레이터 프로토콜을 준수한 이터레이터는 **next 메소드**를 갖는다.

```js
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메소드는 이터레이터를 반환한다.
const iterator = array[Symbol.iterator]();

// 이터레이터 프로토콜을 준수한 이터레이터는 next 메소드를 갖는다.
console.log('next' in iterator); // true
```

<br>

이터레이터의 next 메소드를 호출하면 value, done 프로퍼티를 갖는 **이터레이터 리절트(iterator result) 객체를 반환한다.**

```js
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메소드는 이터레이터를 반환한다.
const iterator = array[Symbol.iterator]();

// 이터레이터 프로토콜을 준수한 이터레이터는 next 메소드를 갖는다.
console.log('next' in iterator); // true

// 이터레이터의 next 메소드를 호출하면 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
// next 메소드를 호출할 때 마다 이터러블을 순회하며 이터레이터 리절트 객체를 반환한다.
console.log(iterator.next()); // {value: 1, done: false}
console.log(iterator.next()); // {value: 2, done: false}
console.log(iterator.next()); // {value: 3, done: false}
console.log(iterator.next()); // {value: undefined, done: true}
```



> #### 참고 및 출처
>
> - [이터레이션과 for...of 문](https://poiemaweb.com/es6-iteration-for-of)
> - 