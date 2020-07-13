---
title: 'RXJS of()'
date: 2019-11-20 16:00:00
category: 'Today I Learned'
draft: false
---

## of operator

⇒ 인수를 연속적인 observable 형태로 변환한다

EX) emitting an object, array, and function

```
  // RxJS v6+
  import { of } from 'rxjs';
  //emits values of any type
  const source = of({ name: 'Brian' }, [1, 2, 3], function hello() {
    return 'Hello';
  });
  
  const subscribe = source.subscribe(val => console.log(val));
  //output: {name: 'Brian'}, [1,2,3], function hello() { return 'Hello' }

  import { of } from 'rxjs';
   
  of(10, 20, 30)
  .subscribe(
    next => console.log('next:', next),
    err => console.log('error:', err),
    () => console.log('the end'),
  );
  // result:
  // 'next: 10'
  // 'next: 20'
  // 'next: 30'
```

> Each argument becomes a next notification.

![https://rxjs.dev/assets/images/marble-diagrams/of.png](https://rxjs.dev/assets/images/marble-diagrams/of.png)
