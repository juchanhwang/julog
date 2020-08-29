---
title: 'Generator'
date: 2020-08-26 16:00:00
category: 'JavaScript'
draft: false
---

## Generator Function

ES6에서 도입된 **Generator Function**는 사용자의 요구에 따라 다른 시간 간격으로 여러 값을 반환할 수 있으며, 내부 상태를 관리할 수 있는 함수이며, `function* generatorFunction() { /* ... */ }`와 같이 사용한다. 즉, **Generator Function은 이터러블을 생성하는 함수이다.** 

단 한 번의 실행으로 함수의 끝까지 실행이 완료되는 일반 함수와는 달리,
제너레이터 함수는 사용자의 요구에 따라 (`yield`와 `next`를 통해) 일시적으로 정지될 수도 있고, 다시 시작될 수도 있다.
제너레이터 함수는 비동기 처리에 유용하게 사용되며, 제너레이터 함수의 반환으로는 제너레이터가 반환된다.

```js
// 이터레이션 프로토콜을 구현하여 무한 이터러블을 생성하는 함수
const createInfinityByIteration = function () {
  let i = 0; // 자유 변수
  return {
    [Symbol.iterator]() { return this; },
    next() {
      return { value: ++i };
    }
  };
};

for (const n of createInfinityByIteration()) {
  if (n > 5) break;
  console.log(n); // 1 2 3 4 5
}

// 무한 이터러블을 생성하는 제너레이터 함수
function* createInfinityByGeneratorFn() {
  let i = 0;
  while (true) { yield ++i; }
}

for (const n of createInfinityByGeneratorFn()) {
  if (n > 5) break;
  console.log(n); // 1 2 3 4 5
}
```

<br>

제너레이터 함수는 일반 함수와는 다른 독특한 동작을 한다. 제너레이터 함수는 일반 함수와 같이 함수의 코드 블록을 한 번에 실행하지 않고 함수 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재시작할 수 있는 특수한 함수이다.

```js
function* counter() {
  console.log('첫번째 호출');
  yield 1;                  // 첫번째 호출 시에 이 지점까지 실행된다.
  console.log('두번째 호출');
  yield 2;                  // 두번째 호출 시에 이 지점까지 실행된다.
  console.log('세번째 호출');  // 세번째 호출 시에 이 지점까지 실행된다.
}

const generatorObj = counter();

console.log(generatorObj.next()); // 첫번째 호출 {value: 1, done: false}
console.log(generatorObj.next()); // 두번째 호출 {value: 2, done: false}
console.log(generatorObj.next()); // 세번째 호출 {value: undefined, done: true}
```

<br>

## Generator

**Generator**는 이 제너레이터 함수의 반환이다. **제너레이터는 이터러블(iterable)이면서 동시에 이터레이터(iterator)인 객체이다.** 다시 말해 제너레이터 함수가 생성한 제너레이터는 Symbol.iterator 메소드를 소유한 이터러블이다. 그리고 제너레이터는 next 메소드를 소유하며 next 메소드를 호출하면 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 이터레이터이다. 즉, 제너레이터의 이터러블에서 반환하는 이터레이터는 자기 자신이다. 

```js
function* generatorFunction() {
  yield 42;
}

const generator = generatorFunction();

generator === generator[Symbol.iterator]();
```

> 이 말인 즉슨, 제너레이터의 이터러블은 다음과 같은 방식으로 구현되어 있을 거라는 것을 암시한다.

```js
generator[Symbol.iterator] = () => this;
```

<br>

### yield / next

`yield`는 제너레이터 함수의 실행을 일시적으로 정지시키며, `yield` 뒤에 오는 표현식은 제너레이터의 caller에게 반환된다.
즉, 일반 함수의 `return`과 매우 유사하다고 볼 수 있다.

여기서 제너레이터 함수는 Callee이고, 이를 호출하는 함수가 Caller이며, Caller는 Callee의 `yield` 부분에서 다음 statement로 진행을 할 지 여부를 제어한다. 이는 `next`로 인해 재개될 수 있다.

`yield`와 `next`의 관계를 보고 이러한 의문이 생길 수 있다. <span style="color:rgb(59, 156, 255)">모든 yield를 처리하기 위해 그만큼의 next를 사용해야 하나?</span>
그럴 수도 있고, 아닐 수도 있다.

`next`를 일일이 호출하지 않고, programmitically하게 호출하게 하려면, 다음과 같이 재귀 호출을 하면 된다.
아래 예제 코드는 홀수는 그대로 출력을 하고, 짝수에는 `1`을 더하여 출력하는 Runner이다.

<br>

### 제너레이터 함수에서의 return

`return`은 수행되고 있는 이터레이터를 종료시키며, `return` 뒤에 오는 값은 <span style="color:rgb(59, 156, 255)">IteratorResult </span> 객체의 `value` 프로퍼티에 할당되며, `done` 프로퍼티는 `true`가 할당된다.

```js
function* sampleGFunction() {
  return 42;
}

const generator = sampleGFunction();
console.log(generator.next()); // { value: 42, done: true }
```

<br>

<br>

> ###  참고 및 출처
>
> - [JavaScript Generator 이해하기 - WONISM](https://wonism.github.io/javascript-generator/)
>
> - [Generator - MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator)
>
> - [제너레이터와 async/awit - POIEMA](https://poiemaweb.com/es6-generator)

