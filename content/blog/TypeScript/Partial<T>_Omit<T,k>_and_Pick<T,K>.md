---
title: "Partial<T>, Omit<T, K> and Pick<T,K>"
date: 2020-07-18 16:00:00
category: 'TypeScript'
draft: false
---



## Partial<**T**>

> 타입 T의 모든 프로퍼티를 Optional 형태로 바꾸어준다.

```ts
type Partial<T> = { [P in keyof T]?: T[P]; };
```

오른쪽에서 **P in keyof T**는 타입 **T**의 프로퍼티 키 값에 해당하는 **P**를 전부 옵셔널(물음표 키워드) 형태로 감싸 리턴한다.

<br>

> ex1)

```ts
interface User {
  name: string;
  age: number;
}

let user1: User = { name: 'harry', age: 23 } // OK
let user2: User = { age: 23 } // ERROR

let user3: Partial<User> = { age: 23 } // OK
```

Partial은 원시 타입에 해당하는 프로퍼티 값을 할당할 수도 안 할 수도 있지만 원시 타입에 존재하지 않는 값은 할당할 수 없다.

<br>

>  ex2)

```ts
interface Todo {
  title: string;
  description: string;
}

function upadateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  // todo, fieldToUpdate 두개 모두 값이 있을 경우 오른쪽 값이 할당됨.
  return { ...todo, ...fieldsToUpdate }
}

const todo1 = {
  title: 'organize desk',
  description: 'clear clutter'
}

const todo2 = updateTodo(todo1, {
  description: 'throw out trash'
})
```

<br>

<br>

## Omit<T, K>

> Pick과는 정반대로 T타입으로부터 K프로퍼티를 제거한다.

```ts
type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>
```

<br>

Pick과 Exclude를 한 곳에 넣고 보면 다소 가독성이  떨어져 이해하기 어려울 수 있는데 아래 코드를 보면 쉽게  이해할 수 있다.

```ts
type Car = {
    name: string;
    price: number;
    brand: string;
};

//Car에 속하는 Key값들중 brand를 키값으로 갖지않는 프로퍼티들을 
//RemainingKey에 넣습니다.
type RemainingKeys = Exclude<keyof Car, "brand">;

//Car에서 RemaningKeys에 속하는 키값에 속하는 프로퍼티들을 리턴합니다 
type NoBrandCard = Pick<Car, RemainingKeys>;
//결과 type NoBrandCard = { name: string; age: number; };

```

> ex1)

```ts
type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>


interface Test {
    a: string;
    b: number;
    c: boolean;
}


type OmitA = Omit<Test, "a">; // Equivalent to: {b: number, c: boolean}
type OmitAB = Omit<Test, "a"|"b">; // Equivalent to: {c: boolean}

```

<br><br>

## Pick<T, K>

> T 타입으로부터 K 프로퍼티만 추출한다.

```ts
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};
```

<br>

키 값 T에 속하는 유니온 타입 K를 받아(= K extends keyof T) 매칭 되는 프로퍼티만 리턴한다.(= [P in K: T[p]])

> ex1)

```ts
interface Todo {
  title: string;
  description: string;
  copleted: boolean;
}

type TodoPreview = Pick<Todo, 'title' | 'completed'>;

const todo: TodoPreview = {
  title: 'Clean room',
  completed" false;
}
```

