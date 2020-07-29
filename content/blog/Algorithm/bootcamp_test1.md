---
title: "bootcamp test1"
date: 2020-07-29 16:00:00
category: 'Algorithm'
draft: false
---



## 문제 설명

자연수가 들어있는 배열 arr가 매개변수로 주어집니다. 배열 arr안의 숫자들 중에서 앞에 있는 숫자들부터 뒤에 중복되어 나타나는 숫자들 중복 횟수를 계산해서 배열로 반환하도록 solution 함수를 완성해주세요. 만약 중복되는 숫자가 없다면 배열에 -1을 채워서 반환하세요.

<br>

### 제한 조건

- 배열 arr의 길이는 1 이상 100 이하의 자연수입니다.
- 배열 arr의 원소는 1 이상 100 이하의 자연수입니다.

<br>

### 입출력 예

- arr = [1, 2, 3, 3, 3, 4, 4]에서 3은 3번, 4는 2번씩 나타나므로 [3, 2]를 반환합니다.

- arr = [3, 2, 4, 4, 2, 5, 2, 5, 5]이면 2가 3회, 4가 2회, 5가 3회 나타나므로 [3, 2, 3]를 반환합니다.
- [3, 5, 7, 9, 1]에서 중복해서 나타나는 숫자는 없으므로 [-1]을 반환합니다.

<br><br>

### 풀이

```js
function solution(arr) {
  arr.sort((a, b) => a - b);
  let count = 1;
  let result = [];
  arr.forEach((element, idx) => {
    if (element === arr[idx + 1]) {
      count++;
    } else if (count !== 1) {
      result.push(count);
      count = 1;
    } 
  });

  if (!result.length) result.push(-1);

  return result;
}

const arr = [1,1,2,3,3,4,4,4];

console.log(solution(arr)); // [ 2, 2, 3 ]
```