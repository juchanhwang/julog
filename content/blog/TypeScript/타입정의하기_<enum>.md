---
title: "타입 정의하기 <enum>"
date: 2020-09-14 16:00:00
category: 'TypeScript'
draft: false
---

- void: 아무것도 반환하지 않고 종료되는 함수의 반환 타입

- never: 항상 예외가 발생해서 비정상적으로 종료되거나 무한루프 때문에 종료되지 않는 함수의 반환 타입

- type: type 키워드를 이용해서 타입에 별칭을 줄 수 있다.

```ts
type Width = number | string;
let width = Width;
width = 100;
width = '100px';
```

<br>

### enum

자바스크립트에는 없고 타입스크립트에만 존재하는 타입. 안의 원소를 타입 뿐만 아니라 값으로도 사용할 수 있다.

enum의 첫 번째 원소에 값을 할당하지 않으면 자동으로 0이 할당된다. enum의 각 원소에는 숫자 또는 문자열을 할당할 수 있는데 명시적으로 값을 입력하지 않으면 이전 원소에서 1만큼 증가한 값이 할당된다. 따라서 Orange는 5에서 1만큼 큰 6이 할당될 것이다.

```ts
enum Fruit {
  Apple,
  Banana = 5,
  Orange
};

const v1: Fruit.Apple;
const v2: Fruit.Apple | Fruit.Banan = Fruit.Banana;

console.log(Fruit.Apple, Fruit.Banana, Fruit.Orange); // 0 5 6
console.log(Fruit.Banana, Fruit['Banana'], Fruit.[5]); // 5 5 Banana
```

> 컴파일 결과

```js
var Fruit;
(funtion (Fruit) {
  Fruit[(Fruit['Apple'] = 0)] = 'Apple';
  Fruit[(Fruit['Banana'] = 5)] = 'Banana';
  Fruit[(Fruit['Orange'] = 6)] = 'Orange';
})(Fruit || Fruit = {}));
```

Fruit라는 변수를 선언해서 객체를 할당하고, 값을 넣고 있다.

<br>

enum 아이템의 값은 숫자 뿐만 아니라 문자열을 입력할 수 있다.	

```ts
enum Language {
  Korean = 'ko';
  English = 'en';
  Japanese = 'jp';
}


// 컴파일

var Language;
(function (Language) {
  Language['Korean'] = 'ko';
  Language['English'] = 'en';
  Language['Japanese'] = 'jp';
})(Language || (Language = {}));
```

enum의 원소에 문자열을 할당하는 경우에는 단방향으로 맵핑 된다.

### enum을 사용해서 유틸리티 함수 구현

```ts
function getIsValidEnumValue(enumObject: any, value: number | string) {
  return Object.keys(enumObject)
    .filter(key => isNaN(Number(key)))
    .some(key => enumObject[key] === value);
};
```

이 함수는 어떤 enum 객체에 특정 value가 있는지 검사하는 함수이다. 먼저, enum 객체에서 모든 키를 뽑아낸 다음 마지막에 enum 객체 안에 입력된 value가 있는지 검사하고 있다. 중간에 filter()를 하는 이유는 **양방향 맵핑** 때문이다.

```ts
enum Some {
  key1 = 1,
};

// Some['key1'] = 1;
// Some[1] = 'key1';
```

이렇듯 양방향으로 맵핑된다. 따라서 value에 1이 입력 됐을 때 true가 되고, value의 key1이 입력됐을 때는 false가 된다.

<br>

enum을 사용하면 컴파일 후에도 enum 객체가 남아있기 때문에 번들 파일의 크기가 불필요하게 커질 수 있다. enum 객체에 접근하지 않는다면, 컴파일 후에 객체로 남겨 놓을 필요가 없을 것이다. `const enum`을 사용해서 컴파일 결과에 enum의 객체를 남겨 놓지 않을 수 있다.

```ts
const enum Fruit {
  Apple,
  Bannana,
  Orange,
}
const fruit: Fruit = Fruit.Apple;

const enum Language {
  Korean: 'ko',
  English: 'en',
  Japanese: 'jp',
}
const lang: Language = Language.Korean;
```

이 코드를 컴파일한 결과이다. enum 객체가 노출되지 않고 사용한 값만 노출하고 있다.

```js
"use strict";
Object.defineProperty(exports, "__esModule", { value: true});
var fruit = 0 /* Apple */;
var lang = "ko" /* Korean */;
```



> ### 참조 및 출처
>
> - [타입스크립트 시작하기](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0/dashboard)

