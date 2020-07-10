---
title: "@types, typeRoots and types"
date: 2020-07-10 16:00:00
category: 'Today I Learned'
draft: false
---

타입스크립트 공식문서에 compilerOptions들로 **typeRoots**와 **types**를 아래와같이 정의한다.

> If `typeRoots` is specified, *only* packages under `typeRoots` will be included.

```json
{
   "compilerOptions": {
	   "typeRoots": [
       "../../node_modules/@types",
       "../../typings"
     ],
   }
}
```

### 

### 

>  If `types` is specified, only packages listed will be included.

```json
{
   "compilerOptions": {
       "types" : ["node", "core"]
   }
}
```

**types에 선언된 패키지들을 typeRoots에 선언된 패키지 하위에서만 탐색할 것이라는 의미이다.** 

예를들어, node, core 패키지들의 인터페이스를 `node_modules/@types`의 하위 폴더, 또 `/typings`의 하위 폴더에서만 탐색 하겠다는 것이다. @types하위 폴더에 존재하지 않으면 typings의 하위 폴더를 탐색한다. 더 구체적인 예시를 살펴보자.

### 

### 

기존 방식은 `project>core>types` 경로에서 필요한 타입을 컴포넌트에 import하는 방식이었다.

> index.ts

```ts
export interface StringKeyDict<T> {
  [key: string]: T;
}
```

### 

> StringKeyDict 인터페이스를 사용하는 컴포넌트

```ts
import { StringKeyDict } from '../../../../../types/src/typings';

selectedCategories: StringKeyDict = {};
```

### 

하지만 우리는 tsconfig compilerOptions의 types에 'core'패키지를 명시했다. 

> tsconfig.app.json

```json
{
   "compilerOptions": {
	   "typeRoots": [
       "../../node_modules/@types",
       "../../typings"
     ],
     "types": ['core']
   }
}
```



즉, core에 선언된 인터페이스를 탐색하겠다는 의미이다. 그리고, 모든 폴더를 탐색하는 것이 아닌, typeRoots에 명시된 패키지들을 순차적으로(node_modules/@types => /typings)탐색 하며 타입을 찾을 것이다.

 `project>typings>core` core경로 하위에 `index.d.ts`파일을 선언한다. 

```ts
export declare global {
  interface StringKeyDict<T> {
    [key: string]: T;
  }
}
```



그렇기 때문에 특정  컴포넌트에서 StringKeyDict 타입을 명시할 때 StringDict가 선언되어있는 파일을 import할 필요가 없어졌다. 

```ts
//  import { StringKeyDict } from '../../../../../types/src/typings';
//  코드 삭제

selectedCategories: StringKeyDict = {};
```

이 방식을 사용함으로서 인터페이스를 매번 임포트해야하는 비용이 줄었다. 또한 직접 임포트하며 탐색하는 메모리 비용이 compilerOptions로 탐색하는 메모리 비용보다 적게 드는 것으로 알고있다(확실하지는 않다..) 

여러 타입을 관리하고 사용해야하는 큰 프로젝트에서는 좋은 경험이 될 것이다.



> ### 참고 및 출처
>
> - [@types, typeRoots and types ](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#types-typeroots-and-types)