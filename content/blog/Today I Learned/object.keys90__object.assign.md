---
title: 'object.keys90 / object.assign'
date: 2019-11-12 16:00:00
category: 'Today I Learned'
draft: false
---



# Object.keys()

`Object.keys()` 메서드는 개체 고유 속성의 이름을 배열로 반환합니다. 배열 순서는 일반 반복문을 사용할 때와 같습니다.

```typescript
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};

console.log(Object.keys(object1));
// expected output: Array ["a", "b", "c"]
```



# Object.assign ([MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign))

Object.assign() 메소드는 열거할 수 있는 하나 이상의 출처 객체로부터 대상 객체로 속성을 복사할 때 사용합니다. 대상 객체를 반환합니다.

```typescript
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```

동일한 키가 존재할 경우 대상 객체의 속성은 출처 객체의 속성으로 덮어쓰여집니다. 후에 출처의 속성은 이전의 출처의 속성과 유사하게 덮어씁니다.

`Object.assign()` 메소드는 열거할 수 있는 출처 객체의 속성만 대상 객체로 복사합니다. 이 메소드는 출처 객체의 `[[Get]]`, 대상 객체의 `[[Set]]` 을 사용하여 getter 와 setter 를 호출합니다. 따라서 이는 속성을 단순히 복사하거나 새롭게 정의하는 것이 아니라 할당하는 것입니다. 병합 출처가 getter 를 포함하는 경우 프로토타입으로 새로운 속성을 병합하는 것이 적절하지 않을 수 있습니다. 프로토타입으로 속성의 열거성을 포함한 속성의 정의를 복사하려면 대신 `[Object.getOwnPropertyDescriptor()]()` 와 `[Object.defineProperty()]()` 를 사용해야합니다.

`[String]()` 과 `[Symbol]()` 속성 모두가 복사됩니다.

에러가 발생할 수 있는 상황에서는(예, 프로퍼티가 쓰기 불가인 경우), `[TypeError]()` 가 발생하며, 에러가 발생하기 전에 속성이 추가되었다면 `target` 객체는 변경될 수 있습니다.

`Object.assign()` 메소드는 `[null]()` 또는 `[undefined]()` 출처 값에 대해서는 오류를 던지지 않음을 유의하세요.



# [How would I implement a filter() for Objects in JavaScript?](https://stackoverflow.com/questions/5072136/javascript-filter-for-objects)

- [How would I implement a filter() for Objects in JavaScript?](https://stackoverflow.com/questions/5072136/javascript-filter-for-objects)

  - [4. ARMD —Delete some key’s from object](https://overflowjs.com/posts/Map-Reduce-Filter-In-Javascript.html)

  With reduce and Object.keys to implement the desired filter (using ES6 arrow syntax):

  ```
    //1 delete
    const newUsers = users.map(u => Object.keys(u)
    									.reduce((newObj, key) => 
    										key != 'website' ?
    										{ ...newObj, [key]: u[key] } :
    										newObj, {}));
    
    //2 include, filter
    const raw = {
      item1: { key: 'sdfd', value:'sdfd' },
      item2: { key: 'sdfd', value:'sdfd' },
      item3: { key: 'sdfd', value:'sdfd' }
    };
    
    const allowed = ['item1', 'item3'];
    
    const filtered = Object.keys(raw)
      .filter(key => allowed.includes(key))
      .reduce((obj, key) => {
        obj[key] = raw[key];
        return obj;
      }, {});
    
    console.log(filtered);
  ```

  - **As (1), in combination with Object.assign**

  In the above solution the comma operator is used in the reduce part to return the mutated res object. This could of course be written as two statements instead of one expression, but the latter is more concise. To do it without the comma operator, you could use Object.assign instead, which does return the mutated object:

  ```
    Object.filter = (obj, predicate) => 
        Object.keys(obj)
              .filter( key => predicate(obj[key]) )
              .reduce( (res, key) => Object.assign(res, { [key]: obj[key] }), {} );
    
    // Example use:
    var scores = {
        John: 2, Sarah: 3, Janet: 1
    };
    var filtered = Object.filter(scores, score => score > 1); 
    console.log(filtered);
  ```

  - **Using map and spread syntax instead of reduce**

  Here we move the Object.assign call out of the loop, so it is only made once, and pass it the individual keys as separate arguments (using the spread syntax):

  ```
    Object.filter = (obj, predicate) => 
        Object.assign(...Object.keys(obj)
                        .filter( key => predicate(obj[key]) )
                        .map( key => ({ [key]: obj[key] }) ) );
    
    // Example use:
    var scores = {
        John: 2, Sarah: 3, Janet: 1
    };
    var filtered = Object.filter(scores, score => score > 1); 
    console.log(filtered);
  ```

  - **Using Object.entries and Object.fromEntries**

  As the solution translates the object to an intermediate array and then converts that back to a plain object, it would be useful to make use of `[Object.entries]()` (ES2017) and the opposite (i.e. [create an object from an array of key/value pairs](https://stackoverflow.com/a/43682482/5459839)) with `[Object.fromEntries]()` (ES2019).

  It leads to this "one-liner" method on `Object`:

  ```
    Object.filter = (obj, predicate) => 
                      Object.fromEntries(Object.entries(obj).filter(predicate));
    
    // Example use:
    var scores = {
        John: 2, Sarah: 3, Janet: 1
    };
    
    var filtered = Object.filter(scores, ([name, score]) => score > 1); 
    console.log(filtered);
  ```
