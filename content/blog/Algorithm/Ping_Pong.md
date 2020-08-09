---
title: "Ping Pong"
date: 2020-08-09 16:00:00
category: 'Algorithm'
draft: false
---



## 문제 설명

> Return a number sequence replacing multiples of the following with words: 3 = Ping 5 = Pong
>
> For example 1 2 Ping 4 Pong
>
> Parameters will be start number and end number

<br>

### 제한 조건

- In the event of a number being a multiple of either insert both words without spaces

<br>

### 입출력 예

| startNumber | endNumber | result                        |
| ----------- | --------- | ----------------------------- |
| 1           | 5         | "1 2 Ping 4 Pong"             |
| 10          | 15        | "Pong 11 Ping 13 14 PingPong" |

<br>

<br>

## 풀이

```ts
export function pingPong(startNumber: number, endNumber: number): string {
let result: string[] = [];

  for (let n: number = startNumber; n <= endNumber; n++) {
    if (n % 15 === 0) result.push('PingPong');
    else if (n % 3 === 0) result.push('Ping');
    else if (n % 5 === 0) result.push('Pong');
    else result.push(String(n));
  }

  return result.join(' ');
}
```

