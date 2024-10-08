---
title: "bootcamp test2"
date: 2020-07-30 16:00:00
category: 'Algorithm'
draft: false
---

## 문제 설명

배열 arr1과 arr2가 주어집니다. 각 배열의 원소는 숫자 1부터 13까지로 이루어져 있습니다.
이 배열에서 2번 이상 페어로 나오는 숫자들을 비교해서 둘 중에 어느 쪽이 더 큰 값인지 확인해야 합니다.
arr1 배열이 더 큰 값을 포함하면 1을, arr2 배열이 더 큰 값을 포함하고 있으면 2를 반환 하는 solution 함수를 완성해 주세요.

양쪽 배열에 모두 페어가 없으면 0을 반환하세요.
같은 숫자 2개가 페어로 나오는 경우보다는 3개가 페어로 나오는 경우가 더 큰 값이다.
같은 숫자 3개가 페어로 나오는 경우보다는 4개가 페어로 나오는 경우가 더 큰 값이다.
같은 숫자 5개이상 페어로 나오는 경우는 4개가 페어로 나오는 경우와 동일하게 취급한다.
양쪽 모두 같은 개수의 페어라면 더 큰 숫자가 큰 값이다.
숫자 페어가 2벌이 나오는 경우에는 더 큰 숫자로 되어있는 페어만 고려한다.
양쪽 배열에서 페어 숫자가 같으면 비교를 할 수 없으니 0을 반환하세요.<br>

### 제한 조건

각 배열 arr의 크기 : 7
각 배열 arr의 원소의 크기 : 1보다 크거나 같고 13보다 작거나 같은 정수

<br>

### 입출력 예

arr1 = [1, 5, 7, 2, 9, 13, 10]이고, arr2 = [2, 3, 9, 10, 4, 8, 11] 이면 페어가 없으니 0를 반환합니다.
arr1 = [1, 4, 1, 3, 5, 6, 10]이고, arr2 = [9, 2, 3, 1, 3, 4, 10] 이면 숫자 3이 페어로 나온 2를 반환합니다.
arr1 = [1, 1, 9, 4, 1, 3, 11]이고, arr2 = [2, 3, 3, 13, 12, 9, 9] 이면 숫자 1이 3번 페어로 나온 1을 반환합니다.
arr1 = [1, 4, 9, 4, 1, 10, 13]이고, arr2 = [11, 13, 9, 3, 1, 9, 1] 이면 페어중에 큰 숫자 9가 나온 2를 반환합니다.
arr1 = [1, 4, 4, 4, 1, 10, 4]이고, arr2 = [11, 13, 11, 3, 11, 9, 1] 이면 4가 4번 페어가 나온 1을 반환합니다.
arr1 = [1,2,2,3,2,2,2]이고, arr2 = [4,5,4,5,4,5,4] 이면 4가 4번 페어가 나온 2를 반환합니다.

solution 함수 이외에 필요한 함수를 추가해서 코드를 의미있는 단위로 분리해보세요.

<br><br>

### 풀이

```js
function solution(arr1, arr2) {
  let pare1 = sidefn(arr1);
  let pare2 = sidefn(arr2);
  let result = 0;
  pare1.sort((a, b) => b - a);
  pare2.sort((a, b) => b - a);

  pare1[0][1] > pare2[0][1] ? (result = 1) : 
  pare2[0][1] > pare1[0][1] ? (result = 2) :
  (result = 0);

  if(pare1[0][1] === pare2[0][1]) {
    if (pare1[0][0] > pare2[0][0]) {
      result = 1
    } else result = 2;
  }

  return result;
}

function sidefn(arr) {
  let remEl = 0;
  arr.sort((a, b) => a - b);
  let count = 1;
  let result = [];
  arr.forEach((el, idx) => {
    if (el === arr[idx + 1]) {
      count++;
    } else if (count !== 1) {
      remEl = el;
      result.push([remEl, count]);

      if (result.length > 1) max(result)
      count = 1;
    }
  });

  return result;
}


function max(arr) {
  let res = 0;  
  if(arr[0][1] > arr[1][1]) arr.pop()
  else arr.shift()
}

const arr1 = [1,1, 3,3,3];
const arr2 = [4,4,4];

console.log(solution(arr1, arr2)); // 2
```



