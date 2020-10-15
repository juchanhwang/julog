---
title: "제네릭(Generic)"
date: 2020-10-09 16:00:00
category: 'TypeScript'
draft: false
---

제네릭은 타입 정보가 **동적**으로 결정되는 타입이다. 제네릭을 통해 같은 규칙을 여러 타입에 적용할 수 있기 때문에 타입 코드를 작성할 때 발생할 수 있는 중복 코드를 제거할 수 있다. 쉽게 말해, 제네릭은 **선언 시점이 아니라 생성 시점에 타입을 명시하여 하나의 타입만이 아닌 다양한 타입을 사용할 수 있도록 하는 기법**이다. 한 번의 선언으로 다양한 타입에 재사용이 가능하다는 장점이 있다.

```ts
function makeNumberArray(defaultValue: number, size: number): number[] {
  const arr: number[] = [];
  for(let i = 0; i < size; i++) {
    arr.push(defaultValue);
  }
  return arr;
}

function makeStringArray(defaultValue: string, size: number): string[] {
  const arr: string[] = [];
  for (let i = 0; i < size; i++) {
    arr.push(defaultValue);
  }
  return arr;
}

const arr1 = makeNumberArray(1, 10);
const arr2 = makeStringArray('empty', 10);
```

`makeNumberArray`함수는 입력된 size만큼의 number array를 반환한다. `makeStringArray`함수도 비슷한 역할을 하는데, 다른 점은 배열에 들어가는 값이 string이라는 것이다. 

> #### 함수 오버로딩(overload)

숫자와 문자만 필요하다면 다음과 같이 **함수 오버로드(overload)**를 사용하면 된다.

```ts
function makeArray(defaultValue: number, size: number): number[];
function makeArray(defaultValue: string, size: numbrer): string[];
```

필요한 타입을 정의하고 로직을 작성하는 것이다.

```ts
function makeArray(
  defaultValue: number | string,
  size: number | string,
): Array<number | string> {
    const arr: Array<number | string>  = [];
    for (let i = 0; i < size; i++) {
      arr.push(defaultValue);
    }
    return arr;
  }

const arr1 = makeArray(1, 10);
const arr2 = makeArray('empty', 10);
```

로직의 중복이 없어졌고 하나의 함수로 사용할 수 있다. 숫자 넘겨줬을 때에는 반환값은 자동으로 숫자고 되고, 문자열을 넘겨줬을 때에는 자동으로 반환값은 문자열이 된다.

> ISSUE: 만약 타입을 추가하고 싶으면 코드를 수정해야한다. 필요한 타입이 늘어나면 많이 번거러울 수 있다.

```ts
function makeArray(defaultValue: number, size: number): number[];
function makeArray(defaultValue: string, size: numbrer): string[];
function makeArray(defaultValue: boolean, size: numbrer): boolean[];

function makeArray(
  defaultValue: number | string | boolean,
  size: number | string | boolean,
): Array<number | string | boolean> {
    const arr: Array<number | string | boolean>  = [];
    for (let i = 0; i < size; i++) {
      arr.push(defaultValue);
    }
    return arr;
  }

const arr1 = makeArray(1, 10);
const arr2 = makeArray('empty', 10);
```

제네릭을 사용하면 간결하게 작성할 수 있다.

```ts
function makeArray<T>(defaultValue: T, size: number): T[] {
  const arr: T[] = [];
  for (let i = 0; i < size; i++) {
    arr.push(defaultValue);
  }
  return arr;
}

const arr1 = makeArray<number>(1, 10);
const arr1 = makeArray<string>('empty', 10);
```

제네릭은 함수 이름 오른쪽에 '<', '>'를 이용해서 입력할 수 있다. `T`라는 것은 원하는 이름으로  정할 수 있고 현재 `T`라는 것의 타입은 정해지지 않은 것이다. `T`의 타입은 동적으로 결정되고 매개변수과 구현하는 부분에서 모두 사용할 수 있다. 

제네릭은 데이터의 타입에 다양성을 부여하기 때문에 자료구조에서 많이 사용된다. 

```ts
class Stack<D> {
  private itmes: D[] = [];
  
  push(item: D) {
    this.items.push(item);
  }
  
  pop() {
    return this.items.pop();
  }
}

const numberStack = new Stack<number>();
numberStack.push(10);
const v1 = numberStack.pop(); // const v1: number | undefined('v1' is declared but its value is never read.) 

const stringStack = new Stack<string>();
stringStack.push('a');
const v2 = stringStack.pop();

let myStack = Stack<number>;
myStack = numberStack;
myStack = stringStack; // error
```

'<', '>' 를 이용해 D라는 이름으로 제네릭을 입력한다. items라는 배열의 타입을 D의 배열로 정의하고 push할 때 D라는 타입의 item을 받아서 그대로 push한다. 사용하는 쪽에서 number의 스택을 만들고 push할 때는 number를 push하고 pop할때는 타입을 보면 number 또는 undefined가 된다.

<br>

제네릭 타입에 아무 타입을 입력할 수 있지만, 리액트와 같은 라이브러리의 API는 입력 가능한 값의 범위를 제한한다. 예를 들어, 리액트의 속성값 전체는 객체 타입만 허용이 된다. 이를 위해 타입스크립트의 제네릭은 타입의 종류를 제한할 수 있는 기능을 제공한다. 다음과 같이 **extends** 키워드를 사용하면 제네릭 타입으로 입력할 수 있는 타입의 종류를 제한할 수 있다.

```ts
function identity<T extends number | string>(p1: T): T {
  return p1;
};

identity(1);
identity('a');
identity([]); // error
```

T라는 타입을 number 또는 string으로 정의했다. 즉,  'A extends B'라는 말은 A가 B에 할당 가능해야한다 라고 이해하면 된다. 그렇기 때문에 T는 number 또는 string에 할당 가능해야한다.

```ts
interface Person {
  name: string;
  age: number;
}

interface Korean extends Person {
  liveInSeoul: boolean; 
}

function swapProperty<T extends Person, K extends keyof Person>(
  p1: T,
  p2: T,
  key: K,
): void {
    const temp = p1[key];
    p1[key] = p2[key];
    p2[key] = temp;
}
```

위의 코드의 함수에서는 `Person`에 할당 가능한 타입과 `keyof Person`에 할당 가능한 제네릭타입을 정의했다. `keyof`라는 것은 오른 쪽에 있는 인터페이스의 모든 속성 이름을 나열한 것이다. 예를들어, `type T1 = keyof Person` 은 `type T1 = "name" | "age"`타입인 것이다. 위의 코드에서는 `K`가 `"name" | "age"`에 할당 가능한 값이어야 하는 것이다. 

```ts
const p1: Korean = {
  name: '홍길동',
  age: 23,
  liveInSeoul: true,
};

const p2: Korean = {
  name: '김삿갓',
  age: 31,
  liveInSeoul: false,
};

swapProperty(p1, p2, 'age');
swapProperty(p1, p2, 'age1'); // error
```

사용하는 코드를 보면, 두 개의 Korean을 만들어서 p1, p2, 그리고 age를 인자로 넘겨주고 있다. 세번째 매게변수에는 name 또는 age를 입력할 수 있다. 이렇게 제네릭을 이용하면 원하는 타입을 제한할 수 있다.

<br>

> ### 참고 및 출처
>
> - [타입스크립트 시작하기](https://www.inflearn.com/course/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0/dashboard)