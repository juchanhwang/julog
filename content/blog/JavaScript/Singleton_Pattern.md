---
title: 'Singleton Pattern'
date: 2020-09-10 16:00:00
category: 'JavaScript'
draft: false
---

## Singleton Pattern(싱글턴 패턴)

객체를 만들 때, 하나의 생성자로 여러 객체를 만들 수 있다. 하지만 싱글턴은 필요에 의해 **단 하나의 객체**만을 만들 때 사용한다. 즉, 전체 시스템에서 하나의 인스턴스만 존재하도록 보장하는 객체 생성 패턴을 말한다.

또한, 싱글턴은 특정 클래스의 인스턴스를 오직 하나만 유지하고, 동일한 클래스로 새로운 객체를 생성해도 처음 만들어진 객체를 얻게 된다. 싱글턴 패턴은 객체지향 언어에서 여러개의 인스턴스 생성을 피하는 유용한 패턴이다. 

js에서의 객체 리터럴이 싱글턴 패턴의 대표적인 예이다. 하지만 모든 속성이 다 공개 되어있기 때문에, **비공개**로 만드는 것이 제대로 된 싱글턴이라고 말한다.

- 객체 리터럴만으로는 비공개 상태나 함수를 정의 할 수 없다. 따라서 외부에서 접근을 할 수 없도록 비공개 멤버를 정의해야 하는데, 여기서 당연히 필요한 개념은 **클로저**이다.
- 즉, 싱글톤 객체를 생성하기 위해선 **객체 리터럴 + 클로저**의 조합이 필요하다.

<br>

### 싱글턴 패턴 예제1

```js
var singleton = (function() {
  var instance;
  var a = 'hello';
  function initiate() {
    return {
      a: a,
      b: function() {
        alert(a);
      }
    };
  }
  return {
    getInstance: function() {
      if (!instance) {
        instance = initiate();
      }
      return instance;
    }
  }
})();

var first = singleton.getInstance();
var second = singleton.getInstance();
console.log(first === second); // true;
```

위에서 볼수 있듯이, **IIFE**로 비공개 멤버 변수를 만들었고, initiate 함수 안에 있는 객체가 실제 객체의 모습을 띄게 된다. 여기서는 a 가 비공개 변수가 되었다.

getInstance 함수를 호출할때, 인스턴스의 유무를 판단하고 없을시에는 initiate함수를 호출하여, 객체를 생성하고 있을시에는 기존 instance를 return한다.

즉, 하나의 인스턴스만 존재하는 **싱글톤 패턴**을 유지하게 된다.

<br>

### 싱글턴 패턴 예제2

```js
var singleton = (function() {

    // 비공개 프로퍼티 정의
    var instance;

    // 비공개 메서드 정의
    function init() {

        // singleton 인스턴스 정의
        return {

            // 공개 프로퍼티 정의
            prop: 'value',

            // 공개 메서드 정의
            method: function() {
                return 'hello'
            }

        };

    }

    // 공개 메서드인 getInstance를 정의한 객체, 비공개 프로퍼티 및 메서드에 접근 가능(클로저)
    return {

        getInstance: function() {

            // 인스턴스가 선언이 안되있는경우 인스턴스 생성
            if (!instance) {
                instance = init();
            }

            // 인스턴스가 선언이 되있는 경우 인스턴스 반환
            return instance;

        }
    }
})()

var singleton1 = singleton.getInstance();
console.log(singleton1.method());
var singleton2 = singleton.getInstance();
console.log(singleton1 === singleton2); // true
```

<br>

### **싱글톤 패턴을 쓰는 이유**

- **메모리 낭비를 방지한다** 
  
  고정된 메모리 영역을 얻으면서 **한번의 new로 인스턴스를 사용하**기 때문
  
- **다른 클래스의 인스턴스들이 데이터를 공유하기 쉬움**

  싱글톤으로 만들어진 클래스의 인스턴스는 **전역 인스턴스**이기 때문

- **DBCP(DataBase Connection Pool)처럼** **공통된 객체를 여러개 생성해서 사용할 때**

  (쓰레드풀, 캐시, 대화상자, 사용자 설정, 레지스트리 설정, 로그 기록 객체등) 안드로이드 앱 같은 경우 각 액티비티나 클래스별로 주요 클래스들을 일일이 전달하기가 번거롭기 때문에 싱글톤 클래스를 만들어 어디서나 접근하도록 설계하는 것이 편하기 때문.


- **인스턴스가** 절대적으로 **한개만 존재하는 것을 보증**하고 싶을 경우
- 두 번째 이용시부터는 객체 로딩 시간이 현저하게 줄어듬

<br>

> ### 참조 및 참고
>
> - [디자인 패턴(싱글턴, 모듈, 생성자)](https://www.zerocho.com/category/JavaScript/post/57541bef7dfff917002c4e86)
>
> - [Javascript - 싱글턴 패턴](https://ideveloper2.tistory.com/160)
>
> - [싱글톤 패턴(Singleton pattern)을 쓰는 이유와 문제점](https://jeong-pro.tistory.com/86)
>
> - [싱글톤 패턴(Singleton Pattern)을 쓰는 이유](https://coding-restaurant.tistory.com/144)
