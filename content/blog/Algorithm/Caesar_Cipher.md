---
title: "Caesar Cipher(시저암호)"
date: 2020-07-14 16:00:00
category: 'Algorithm'
draft: true
---



## 문제 설명

> 어떤 문장의 각 알파벳을 일정한 거리만큼 밀어서 다른 알파벳으로 바꾸는 암호화 방식을 시저 암호라고 합니다. 예를 들어 AB는 1만큼 밀면 BC가 되고, 3만큼 밀면 DE가 됩니다. z는 1만큼 밀면 a가 됩니다. 문자열 s와 거리 n을 입력받아 s를 n만큼 민 암호문을 만드는 함수, solution을 완성해 보세요.

<br>

### 제한 조건

- 공백은 아무리 밀어도 공백입니다.
- s는 알파벳 소문자, 대문자, 공백으로만 이루어져 있습니다.
- s의 길이는 8000이하입니다.
- n은 1 이상, 25이하인 자연수입니다.

<br>

### 입출력 예

| s     | n    | result |
| ----- | ---- | ------ |
| AB    | 1    | BC     |
| z     | 1    | a      |
| a B z | 4    | e F d  |

<br>

<br>

## 풀이

```js
function solution(s, n) {
  let arr = s.split("");
  let answer = "";
  let codeOfAlphabet;
  arr.forEach(element => {
      codeOfAlphabet = element.charCodeAt() + n;
      let movedStr = String.fromCharCode(codeOfAlphabet)
      if (codeOfAlphabet >= 65 && codeOfAlphabet <= 90 || codeOfAlphabet >= 97 && codeOfAlphabet <= 122) {
        answer += movedStr;
    } else if(codeOfAlphabet >= 90 || codeOfAlphabet >= 122) {
        answer += String.fromCharCode(codeOfAlphabet - 26);
    } else answer += " ";
  });
  return answer;
}
```

