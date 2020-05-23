---
title: 'Closure'
date: 2020-05-18 16:00:00
category: 'development'
---



## 클로저란?

**MDN**: A closure is the combination of a function and the lexical environment within which that function was declared

=> 클로저는 함수와 함수가 선언된 어휘적 환경의 조합이다.

=> 클로저는 함수와 그 함수가 선언되던 당시에 정보를 담은 **lexical 환경**의 조합이다.

이 문장을 이해하려면 lexical 환경의 의미를 파악하는 것이 중요하다. 

- lexical: 어휘의, 사전적인 

- environment: 환경

어휘적 환경 혹은 사전적인 환경이라는 뜻이다. 다시 원문을 해석해보면 이렇다. 함수와 "그 함수가 **선언될 당시**의 환경정보" 사이의 조합이다. '선언될 당시'하면 떠오르는 것이 무엇일까.

=> ''선언될 당시'' => SCOPE

클로저는 scope와 밀접한 관련이 있다. scope는 변수의 유효범위이고, 클로저는 그 유효범위로 인한 , 상태다. 다시 한번 정리해보면, 클로저란 함수 내부에서 생성한 **데이터**와 그 **유효범위**로 인해 발생하는 특수한 **현상/상태**다.

사실 클로저는 원래부터 본 의미를 잘 담고 있다. 사전에서 클로저를 검색하면 '닫혀있음', '폐쇄성', '완결성'이라고 나온다. 클로저, 스코프는 폐쇄적이다. 스코프에서 외부에 정보를 제공할 수 있는 유일한 수단은 해당 정보를 return하는 것 뿐이다. 설령 함수를 리턴한다고 해도 그 리턴된 함수 역시 변하지 않는 것이 있다. 바로 최초 선언시에 생성된 스코프와 생성지인 환경정보들이다. **최초 선언시의 정보를 유지한다.** 이 것이 핵심이고 클로저의 전부이다.



## 클로저의 활용

### 접근 권한 제어 & 지역변수 보호

```js
function a() {
  var x = 1;
  function b() {
    console.log(x);
  }
  b();
}
a();
console.log(x)
```

 변수의 유효범위인 스코프를 살펴보면 함수 a의 스코프 내부에 또 다른 함수 b의 스코프가 형성되어 있을 때, a에서 선언된 변수 x는 a의 외부에서는 접근할 수 없다. 그러나 객체 지향 프로그래밍은 외부와의 데이터 연동이 매우 활발히 이루어져야한다. 함수 내부에서만 활용하면 한계가 있다.  예제를 아래와 같이 바꾸어보겠다.

### 데이터 보존 및 활용

```js
function a() {
  var x = 1;
  return function b() {
		console.log(x);
  }
}
var c = a();
console.log(c());
```

a에서 b함수를 반환했고, 외부 c변수에 할당했다. 이제는 c라는 변수를 이용하면 외부에서 x값을 출력할 수 있다.

외부에서 임의로 x값을 바꿀 수 없게 되었다. 외부에서 임의로 x를 바꾸려면 a함수 내부에서 외부에 바꿀 수 있는 수단(권한)을 제공해줘야한다.

아래의 코드를 살펴보라.

### 접근 권한 제어, 데이터 보존 및 활용 & 지역변수 보호

```js
function a() {
  var _x = 1;
  return {
    get x() { return _x; },
    set x(v) { _x = v; }
  }
}

var c = a();
c.x = 10;
```

함수를 리턴하는 대신에 객체에 getter와 setter를 담아서 리턴할 것이다. 외부에서도 x라는 프로퍼티 값을 변경할 수 있다. 이처럼 내부에서 반환을 통해서 권한을 주면 외부에서는 부여받은 권한만을 이용해서 내부와 소통할 수 있다.

_x라는 변수명은 외부로 전혀 노출되어있지 않다. getter와 setter를 어떻게 세팅하느냐에 따라서 무엇을 리턴할지를 a 함수가 결정하는 것이다.

#### ex 1)

```js
function setName(name) {
  return function() {
    return name;
  }
}

var sayMyName = setName('고무곰');
sayMyName();
```

<img width="700" alt="스크린샷 2020-05-18 오후 10 45 41" src="https://user-images.githubusercontent.com/36187948/82220408-88886280-9959-11ea-917b-879c5fdc0d34.png">



> 참고
>
> - [Javascript 핵심 개념 알아보기](https://www.inflearn.com/course/%ED%95%B5%EC%8B%AC%EA%B0%9C%EB%85%90-javascript-flow)

