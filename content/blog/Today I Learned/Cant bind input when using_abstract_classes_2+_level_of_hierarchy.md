---
title: "Cant bind input when using abstract classes 2+ level of hierarchy"
date: 2020-04-24 16:00:00
category: 'Today I Learned'
draft: false
---



로컬 컴파일 시점에서는 아무 문제 없는 코드가 배포한 프로덕션에서 에러가 난다.😭

당황스럽지 않을 수가 없다... ㅎㅎ

빌드를 하게 되면 쓰지 않는 코드들을 자동으로 삭제하는데, AbstractComponent 안에 있는 input값이 사용되지 않는다고 판단해서 없앤 것 같다.

### 문제의 코드

- 추상 컴포넌트: AbstractComponent

```ts
export abstract class AbstractComponent {
  @Input() input$: Observable<string>;
  @Input() keyEvent$: Observable<KeyboardEvent>;
  @Input() isFocused$: BehaviorSubject<boolean>;

  ngOnInit() {
    this.keyEvent$.pipe().....
  }
```

- AbstractComponent를 extends하고 있는 컴포넌트

```ts
export class PCSearchAutoComponent extends AbstractComponent {
......
}
```

```ts
export class PCSearchHistoryComponent extends AbstractComponent {
......
}
```

### 에러 메시지

- can't bind to keyEvents$ since it isn't a known property of mp-search-history
- ERROR TypeError: Cannot read property 'pipe' of undefined

## 해결 방안

1. 자식 컴포넌트에 input 값들을 선언

```ts
export class PCSearchAutoComponent extends SearchTypeaheadInteraction {
  @Input() input$: Observable<string>;
  @Input() keyEvent$: Observable<KeyboardEvent>;
  @Input() isFocused$: BehaviorSubject<boolean>;
}
```

```ts
export class PCSearchHistoryComponent extends SearchTypeaheadInteraction {
  @Input() input$: Observable<string>;
  @Input() keyEvent$: Observable<KeyboardEvent>;
  @Input() isFocused$: BehaviorSubject<boolean>;
}
```

- 각 자식 컴포넌트들에게도 `Input` 값들을 선언해준다.



2. 공통 컴포넌트에 @Directive() 데코레이터 선언

```ts
@Directive()
export abstract class AbstractComponent {
  @Input() input$: Observable<string>;
  @Input() keyEvent$: Observable<KeyboardEvent>;
  @Input() isFocused$: BehaviorSubject<boolean>;
}
```

- AbstractComponent에 데코레이터만 추가해주면 쉽게 해결 할 수 있다.
- Angular9부터 Angular가 Abstract class가 올바르게 상속할 수 있다는 것을 알 수 있게, @Directive() 데코레이터를 사용해야한다.

> 참고

- [Angluar Github](https://github.com/angular/angular/issues/35295)
