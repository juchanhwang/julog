---
title: "@types, typeRoots and types"
date: 2020-07-10 16:00:00
category: 'Today I Learned'
draft: true
---



If `typeRoots` is specified, *only* packages under `typeRoots` will be included.

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



If `types` is specified, only packages listed will be included.

```json
{
   "compilerOptions": {
       "types" : ["node", "core"]
   }
}
```

types 내에 선언되어있는 패키지들을 typeRoots에 선언되어있는 패키지에서 탐색을 하는 것이다.

예를들어, node, core 패키지들의 타입을 `node_modules/@types`의 하위 폴더, 또 `/typings`의 하위 폴더에서 탐색을 하겠다는 것이다. 더 구체적인 예시를 살펴보자.



기존 방식은 `project>core>types` 경로에서 필요한 타입을 컴포넌트에 import하는 방식이었다.

```ts
import { StringKeyDict } from '../../../../../types/src/typings';

selectedCategories: StringKeyDict = {};
```



하지만 우리는 ts.config파일에 compilerOptions의 types에 'core'패키지를 명시했다. core에 선언된 타입을 컴파일하겠다는 의미이다.

그리고 이 core패키지는 typeRoots에 명시된 패키지들을 순차적으로(node_modules/@types => /typings) 탐색을 하며 타입을 찾는다.

 `project>typings>core` core경로 하위에 `index.d.ts`파일을 선언한다. 

```ts
export declare global {
  interface StringKeyDict<T> {
    [key: string]: T;
  }
}
```

그리고, 필요한 인터페이스를 선언한다. 



`project>core>types` 폴더 내에 있는 





> ### 참고 및 출처
>
> - [@types, typeRoots and types ](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html#types-typeroots-and-types)