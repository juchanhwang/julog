---
title: "The Baum-Sweet sequence"
date: 2020-08-17 16:00:00
category: 'Algorithm'
draft: false
---

## 문제 설명

> The Baum–Sweet sequence is an infinite automatic sequence of `0`s and `1`s defined by the rule:

<br>

> bn = `1` if the binary representation of `n` contains no block of consecutive `0`s of odd length;
> bn = `0` otherwise;
>
> for `n ≥ 0`.
>
> Define a generator function `baumSweet` that sequentially generates the values of this sequence.

<br>

<br>

## 풀이

```ts
const getBaumSweet = (n) => {
  let binaryList = [];
  let res = 0;

  while (n !== 1) {
    binaryList.push(n % 2);
    n = Math.floor(n / 2);
  }
  binaryList.push(n);
  binaryList.reverse();

  for (let i = 0; i < binaryList.length - 1; i++) {
    switch (binaryList[i]) {
      case 0:
        res++;
        break;
      case 1:
        if (res === 1) break;
    }
    if (res % 2 === 0) res = 0;
  }
  return res === 0;
};
```
