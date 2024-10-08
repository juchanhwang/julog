---
title: 'Activated Route의 구독과 해제'
date: 2020-01-10 16:00:00
category: 'Angular'
draft: false
---

- Angular Activated Route

  컴포넌트에서 옵저버블을 구독하면 이 컴포넌트가 종료될 때 구독했던 옵저버블을 해지하는 것이 좋다고 알고 있을 것이다.

  하지만 이런 로직이 필요하지 않은 경우가 있다. `[ActivatedRoute]()` 옵저버블을 사용하는 경우도 이런 예외에 해당된다.

  `[ActivatedRoute]()`와 이 서비스가 제공하는 옵저버블은 모두 `[Router]()`가 직접 관리합니다. 그래서 `[Router]()`가 라우팅 대상 컴포넌트를 종료하면 이 컴포넌트에 주입되었던 `[ActivatedRoute]()`도 함께 종료된다.

  옵저버블을 해제하지 않았다고 걱정하지 마라. 프레임워크가 알아서 처리할 것이다.

