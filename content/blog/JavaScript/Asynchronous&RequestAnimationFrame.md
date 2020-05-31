---
title: 'Asynchronous & RequestAnimationFrame'
date: 2019-10-22 16:21:13
category: 'JavaScript'
---



## what is Asynchronous?

![](<https://poiemaweb.com/img/block_nonblock.png>)

## asynchronous

1) setTimeOut

- 정해진 시간 이후에 콜백함수를 비동기적으로 실행해줄 것.

2) setInterval

- 정해진 시간을 반복적으로 콜백함수를 비동기적으로 실행해준다.





## **동기-비동기 순서 이해하기**

```js
function plus() {
  let a = 1;
  setTimeout( ()=>console.log(++a), 1000);
  return a;
}

const result = plus();
console.log('result :', result);  //?
```



## **비동기 상황 예**

```js
const baseData = [1,2,3,4,5,6,100];

const asyncRun = (arr, fn) => {
 for(var i=0; i<arr.length; i++) {
   setTimeout( () => fn(i), 1000);
 }
}

asyncRun(baseData, idx =>console.log(idx));
```



## **비동기 상황 예 - forEach로 변경해보자.**

```js
const baseData = [1,2,3,4,5,6,100];
const asyncRun = (arr, fn) => {
   arr.forEach((v,i) => {
     setTimeout( () => fn(i), 1000);
   });
}
asyncRun(baseData, idx =>console.log(idx))
```



## **비동기 상황 예 - 동기 + 비동기 + 동기**

```js
const baseData = [1,2,3,4,5,6,100];

function sync() {
  baseData.forEach((v,i) => {
    console.log("sync ", i);
  });
}

const asyncRun = (arr, fn) => {
   arr.forEach((v,i) => {
     setTimeout( () => fn(i), 1000);
   });
}


function sync2() {
  baseData.forEach((v,i) => {
    console.log("sync 2 ", i);
  });
}

asyncRun(baseData, idx =>console.log(idx))
sync();
sync2();

```



## **비동기 상황 예 - 비동기 + 비동기**

```js
//순서를 예상해보기.
//call stack과 callback queue를 상상하자.

const baseData = [1,2,3,4,5,6,100];

const asyncRun = (arr, fn) => {
   arr.forEach((v,i) => {
     setTimeout(() => {
       setTimeout(() => {
         console.log("cb 2");
         fn(i)
        },1000);
       console.log("cb 1");
     }, 1000);
   });
}

asyncRun(baseData, idx =>console.log(idx))

```



## "this" 문제

`setTimeout()`에 매개변수(혹은 다른 함수)를 전달할 때, 당신의 기대와는 다르게 this의 값이 호출된다.

`setTimeout()`에 의해 실행된 코드는 별도의 실행 컨텍스트에서 `setTimeout`이 호출된 함수로 호출됩니다.  호출된 함수에 대해서는 `this` 키워드를 설정하는 일반적인 규칙이 적용되며, this를 설정 혹은 할당하지 않은 경우, non-strict 모드에서 전역(혹은 `window`) 객체, strict모드에서 undefined를 기본 값으로 합니다. 다음 예제를 봐주세요: 



## window.requestAnimationFrame

window.requestAnimationFrame()은 브라우저에게 수행하기를 원하는 애니메이션을 알리고 다음 리페인트가 진행되기 전에 해당 애니메이션을 업데이트하는 함수를 호출하게 합니다. 이 메소드는 리페인트 이전에 실행할 콜백을 인자로 받습니다. (MDN)

> 다음 리페인트에서 그 다음 프레임을 애니메이트하려면 콜백 루틴이 반드시 스스로 requestAnimationFrame()을 호출해야합니다.

```
var start = null;
var element = document.getElementById('SomeElementYouWantToAnimate');
element.style.position = 'absolute';

function step(timestamp) {
  if (!start) start = timestamp;
  var progress = timestamp - start;
  element.style.left = Math.min(progress / 10, 200) + 'px';
  if (progress < 2000) {
    window.requestAnimationFrame(step);
  }
}

window.requestAnimationFrame(step);
```

### [**The requestAnimationFrame Guide**](https://flaviocopes.com/requestanimationframe/)

프레임워크 또는 샘플에서 setTimeout 또는 setInterval을 사용하여 애니메이션과 같은 시각적 변화를 수행할 수도 있지만, 이 경우 문제는 콜백이 프레임에서 특정 시점(종료 시)에 실행되고, 종종 프레임이 누락되어 버벅거림 현상이 발생할 수 있다는 점입니다.

![img](https://developers.google.com/web/fundamentals/performance/rendering/images/optimize-javascript-execution/settimeout.jpg?hl=ko)

이에 반면 requestAnimationFrame()은 프레임 누락없이 콜백이 실행된다.

![img](https://flaviocopes.com/requestanimationframe/timeline-requestanimationframe.png)

관련 영상: [Jake Archibald: In The Loop](https://www.youtube.com/watch?v=cCOL7MC4Pl0)