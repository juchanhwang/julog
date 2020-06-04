---
title: 'Data Types'
date: 2020-06-04 16:00:00
category: 'JavaScript'
---



자바스크립트 데이터 타입은 크게 두 가지로 분류된다.

<img width="800" alt="스크린샷 2020-06-04 오후 7 09 16" src="https://user-images.githubusercontent.com/36187948/83744358-fb524700-a696-11ea-9fb1-80892a061a5a.png">



- Primitive Type

```js
var b = 'abc';
```

1. 데이터가 담길 공간을 확보
2. 데이터의 주소값을 가지고 변수명 b와 매칭
3. 매칭된 @314주소로 이동해 문자열 값 'abc'를 저장

<img width="800" alt="스크린샷 2020-06-04 오후 7 13 51" src="https://user-images.githubusercontent.com/36187948/83745709-ebd3fd80-a698-11ea-905b-c75d13984ba0.png">



- Reference Type

```js
var obj = {
  a: 1,
  b: 'b'
};
```

1. 데이터가 담길 공간을 확보
2. 데이터의 주소값을 가지고 변수명 obj와 매칭
3. 참조형 데이터의 공간을 새로 확보
4. 'a'프로퍼티와 'b' 프로퍼티를 담을 새로운 공간을 확보
5. 매칭된 주소에 @1012 = 1 @1013 = 'b' 값 할당

<img width="800" alt="스크린샷 2020-06-04 오후 7 20 25" src="https://user-images.githubusercontent.com/36187948/83745711-ed052a80-a698-11ea-9ab2-f670d3b69f21.png">



```js
var obj2 = obj
```

1. 데이터가 담길 공간 확보
2. 주소를 할당

<img width="800" alt="스크린샷 2020-06-04 오후 7 27 38" src="https://user-images.githubusercontent.com/36187948/83746065-7e749c80-a699-11ea-978a-7b711e7d6398.png">



두 데이터 타입 모두 공간을 확보하고 해당 공간의 주소를 변수명과 매칭시키는 과정, **선언과정**과 해당 변수가 기리키는 주소의 공간에 데이터를 저장하는 과정, **할당과정**을 따른다.



> 출처:
>
> - [Javascript 핵심 개념 알아보기](https://www.inflearn.com/course/%ED%95%B5%EC%8B%AC%EA%B0%9C%EB%85%90-javascript-flow)
>

