---
title: "Calculate mean and concatenate string"
date: 2020-08-07 16:00:00
category: 'Algorithm'
draft: false
---

## 문제 설명

> You will be given an array which will include both integers and characters.
>
> Return an array of length 2 with a[0] representing the mean of the ten integers as a floating point number. There will always be 10 integers and 10 characters. Create a single string with the characters and return it as a[1] while maintaining the original order.



### 입출력 예

```ts
lst = ['u', '6', 'd', '1', 'i', 'w', '6', 's', 't', '4', 'a', '6', 'g', '1', '2', 'w', '8', 'o', '2', '0']
```

#### result

```ts
[3.6, "udiwstagwo"]
```



## 풀이

```ts
export function mean(lst: string[]): [number, string] {
  let sum: number = 0;
  let str: string = '';
  for (let i = 0; i < lst.length; i++) {
    let currEl: string = lst[i];
    if (isNaN(Number(currEl))) {
      str = str + currEl;
    } else {
      sum = sum + +currEl;
    }
  }

  return [sum / 10, str];
}
```

> Test

```ts
const test1 = mean(["u", "6", "d", "1", "i", "w", "6", "s", "t", "4", "a", "6", "g", "1", "2", "w", "8", "o", "2", "0"]);
// result: [ 3.6, 'udiwstagwo' ]

const test2 = mean(["0", "c", "7", "x", "6", "2", "3", "5", "w", "7", "0", "y", "v", "u", "h", "i", "n", "u", "0", "0"]);
// result: [ 3, 'cxwyvuhinu' ]

const test3 = mean(["0", "u", "a", "y", "0", "a", "9", "q", "3", "v", "g", "7", "6", "4", "y", "d", "8", "6", "0", "d"]);
// result: [ 4.3, 'uayaqvgydd' ]

const test4 = mean(["s", "n", "9", "l", "0", "m", "i", "z", "9", "7", "y", "4", "z", "3", "3", "k", "4", "1", "0", "k"]);
// result: [ 4, 'snlmizyzkk' ]

const test5 = mean(["5", "v", "u", "k", "8", "4", "9", "b", "9", "g", "5", "z", "3", "f", "6", "u", "i", "6", "6", "t"]);
// result: [ 6.1, 'vukbgzfuit' ]

const test6 = mean(["1", "1", "1", "1", "1", "1", "1", "1", "1", "0", "a", "a", "d", "d", "g", "q", "u", "v", "y", "y"]);
// result: [ 0.9, 'aaddgquvyy' ]

const test7 = mean(["1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "a", "a", "d", "d", "g", "q", "u", "v", "y", "y"]);
// result: [ 1, 'aaddgquvyy' ]
```

