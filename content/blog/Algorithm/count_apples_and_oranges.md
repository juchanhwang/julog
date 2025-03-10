---
title: "count apples and oranges(사과와 오렌지)"
date: 2020-07-14 16:00:00
category: 'Algorithm'
draft: false
---

## 문제 설명

```
/*
* 사과와 오렌지
*
다음과 같은 흑백 세상에 오렌지 나무와 사과나무가 있습니다. a는 사과나무의 위치를, b는 오렌지 나무의 위치를 나타냅니다.
두 나무 사이에는 Vanilla House가 있고, Vanilla House의 영역은 s에서부터 t까지로 정의됩니다.
*
*     * *                                   ___________                                    * *
*     * *                                   | Vanilla |                                    * *
*      |                                    |  House  |                                     |
* -----a---------------------------------s---------------t----------------------------------b-----
* 나무에서 과일이 다 익으면, 나무에서부터 D만큼 떨어진 곳에 떨어지게 됩니다. D가 음수일 경우 나무의 왼쪽으로 떨어지는 것을 의미합니다.
* D가 양수일 경우 과일은 나무의 오른쪽 방향으로 떨어집니다.
* m개의 사과와 n개의 오렌지가 익어서 떨어졌을 때, Vanilla House 영역 안에 떨어진 사과와 오렌지가 몇 개인지 배열로 반환해주세요!
* 반환값은 [사과의 개수, 오렌지의 개수] 형태의 배열로 반환해주시면 됩니다!
*
* 예를들어, Vanilla House가 s=7 이고 t=10 인 영역에 있고, 사과나무와 오렌지 나무가 각각 a=4, b=12인 지점에 있습니다.
* 그리고 m=3개의 사과와 n=3개의 오렌지가 있습니다. 각각 apples=[2, 3, -4]와 oranges=[3, -2, -4]로 떨어지는 방향이 정해졌습니다.
* 그렇다면 사과는 [4 + 2, 4 + 3, 4 + (-4)]=[6, 7, 0]로 떨어지는 지점이 정해지고,
* 오렌지는 [12 + 3, 12 + (-2), 12 + (-4)]=[15, 10, 8]로 떨어지는 지점이 정해집니다.
* 결과적으로 Vanilla House의 영역[7, 10]에는 1개의 사과와 2개의 오렌지가 떨어지게 됩니다. 따라서 [1, 2]를 반환해주면 되겠죠!
*
* @param {number} s
* @param {number} t
* @param {number} a
* @param {number} b
* @param {number[]} apples
* @param {number[]} oranges
* @return {number[]}
*/

// 아래의 export default 키워드는 '아직' 신경쓰지 않으셔도 됩니다. :)
function countApplesAndOranges(s, t, a, b, apples, oranges) {
  let m = 3, n = 3;
  let appleLocation = apples.map(el => el + a);
  let orangeLocation = oranges.map(el => el + b);
  let houseLocation = [];
  for (let i = s; i <= t; i++) {
    houseLocation.push(i);
  }

  let appleInHouseNum = countFruitLocation(appleLocation, houseLocation);
  let orangeInHouseNum = countFruitLocation(orangeLocation, houseLocation);
  
  return [appleInHouseNum, orangeInHouseNum];
}

function countFruitLocation(fruitLocation, houseLocation) {
  let countNum = 0;
  fruitLocation.forEach(element => houseLocation.includes(element) ? countNum++ : null)
  return countNum;
}

console.log(countApplesAndOranges(7, 10, 4, 12, [2, 3, -4], [3, -2, -4]));
```

<br><br>

### 풀이

```js
// 아래의 export default 키워드는 '아직' 신경쓰지 않으셔도 됩니다. :)
function countApplesAndOranges(s, t, a, b, apples, oranges) {
  let m = 3, n = 3;
  let appleLocation = apples.map(el => el + a);
  let orangeLocation = oranges.map(el => el + b);
  let houseLocation = [];
  for (let i = s; i <= t; i++) {
    houseLocation.push(i);
  }

  let appleInHouseNum = countFruitLocation(appleLocation, houseLocation);
  let orangeInHouseNum = countFruitLocation(orangeLocation, houseLocation);
  
  return [appleInHouseNum, orangeInHouseNum];
}

function countFruitLocation(fruitLocation, houseLocation) {
  let countNum = 0;
  fruitLocation.forEach(element => houseLocation.includes(element) ? countNum++ : null)
  return countNum;
}

console.log(countApplesAndOranges(7, 10, 4, 12, [2, 3, -4], [3, -2, -4]));
// [ 1, 2 ]
```

