---
title: 'How to debug on whether observable is unsubscribe(20200318)'
date: 2020-03-18 16:00:00
category: 'Today I Learned'
draft: false
---



```ts
const sub$ = new Subject();
sub$.unsubscribe();
sub$.closed //true;
```

