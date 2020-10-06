---
title: "제네릭(Generic)"
date: 2020-10-04 16:00:00
category: 'TypeScript'
draft: true
---

제네릭은 타입 정보가 **동적**으로 결정되는 타입이다. 제네릭을 통해 같은 규칙을 여러 타입에 적용할 수 있기 때문에 타입 코드를 작성할 때 발생할 수 있는 중복 코드를 제거할 수 있다.

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

숫자와 문자만 필요하다면 다음과 같이 함수 오버로드(overload)를 사용하면 된다.

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

제네릭은 함수 이름 오른쪽에 '<', '>'를 이용해서 입력할 수 있다. `T`라는 것은 원하는 이름으로 정할 수 있고 현재 `T`라는 것의 타입은 정해지지 않은 것이다. `T`의 타입은 동적으로 결정되고 매개변수과 구현하는 부분에서 모두 사용할 수 있다.