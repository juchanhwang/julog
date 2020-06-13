---
title: 'Callback Function'
date: 2020-04-18 16:00:00
category: 'JavaScript'
draft: false
---



# 콜백 함수 (Callback Function)

call + back: something will **call** this function **back** sometime somehow

무언가가 이 함수를 호출해서 언젠가 어떻게든 나에게 돌려줄거야!  => **제어권**을 넘겨준다. 맡긴다.



ex)

```js
setInterval(function () {
  console.log('1초마다 실행될 겁니다.')
}, 1000)
```

- 1초마다 콜백함수가 실행된다. 콜백함수의 제어권을 setInterval에게 넘긴 것이다. 

```js
setInterval(callback, milliseconds)
```

##### > 다른 함수 (A)의 매개변수로 콜백함수 (B)를 전달하면, A가 B의 **제어권**을 갖게 된다.

##### > 특별한 요청(bind)이 없는 한 A에 미리 정해진 방식에 따라 B를 호출한다.

##### > 미리 정해진 방식이란 this에 무엇을 바인딩할지, 매개변수에는 어떤 값들을 지정할지, 어떤 타이밍에 콜백을 호출할지 등이다.



> 참고
>
> - [Javascript 핵심 개념 알아보기](https://www.inflearn.com/course/%ED%95%B5%EC%8B%AC%EA%B0%9C%EB%85%90-javascript-flow)

