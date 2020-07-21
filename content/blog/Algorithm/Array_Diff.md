---
title: "Array Diff"
date: 2020-07-21 16:00:00
category: 'Algorithm'
draft: false
---



```js
function array_diff(a, b) {
  return a.filter(v => !b.includes(v))
}

console.log(array_diff([], [4, 5])); // []
console.log(array_diff([3, 4], [3])); // [ 4 ]
console.log(array_diff([1, 8, 2], [])); // [ 1, 8, 2 ]
```

