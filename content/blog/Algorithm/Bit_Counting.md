---
title: "Bit Counting"
date: 2020-07-27 16:00:00
category: 'Algorithm'
draft: false
---



```js
var countBits = function (n) {
  let sum = 0;
  while (n > 0) {
    n % 2 === 1 ? sum++ : null
    n = parseInt(n / 2);
  }

  return sum;
};

function test(input, expect) {
  const result = countBits(input);
  console.log(`input: ${input}, result: ${result}, expect: ${expect}`)
  return (result === expect) ? console.log("pass!") : console.log("fail!")
}

test(0, 0); // input: 0, result: 0, expect: 0 pass!
test(4, 1); // input: 4, result: 1, expect: 1 pass!
test(7, 3); // input: 7, result: 3, expect: 3 pass!
test(9, 2); // input: 9, result: 2, expect: 2 pass!
test(10, 2); // input: 10, result: 2, expect: 2 pass!
```

