---
title: 'test code 작성 spyOn()(20200107)'
date: 2020-01-07 16:00:00
category: 'Today I Learned'
draft: false
---

- test code 작성 spyOn()

  ```tsx
  // chargeCash가 호출되고 있는지 감시하는 로직
  spyOn(GTagManager, 'chargeCash');
  
  // check result
  expect(GTagManager.chargeCash).toHaveBeenCalled();
  ```

