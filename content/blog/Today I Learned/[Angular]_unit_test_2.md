---
title: "[Angular] Unit Test 2"
date: 2020-10-07 16:00:00
category: 'Today I Learned'
draft: false
---

부모로부터 상속받은 특정 값을 테스트하기 위해서는 동일한 상황의 테스트용 컴포넌트를 만들어야한다.

우선 테스트 하려고하는 컴포넌트를 만들어보자. 그리고, AbstractComponent를 extend한다. AbstractComponent에는 나중에 테스트할 때 사용할 값들을 가지고 있다.

```ts
@Component({
  selector: 'test',
  template: ``,
})
class TestComponent extends AbstractComponent {
}
```

그리고, 이 컴포넌트를 사용하는 부모 컴포넌트를 만든다. calculator 값을 넘겨주고 있다.

```ts
@Component({
  selector: 'host-component',
  template: `
    <test #child [calculator]="calculator"></test>
  `,
})
class HostComponent {
  calculator: ICashCalculator = new KRWCashCalculator();
  @ViewChild('child') childComponent: TestComponent;
}
```

필요한 값들을 선언해주고, `ViewChild`로 가져온 값을 활용해 테스트를 진행한다.

```ts
describe('abstractClass', () => {
  let component: HostComponent;
  let fixture: ComponentFixture<HostComponent>;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [
        TestComponent,
        HostComponent,
      ]
    }).compileComponents();
    fixture = TestBed.createComponent(HostComponent);
    component = fixture.componentInstance;
  });

  it('should be created', () => {
    expect(component).toBeTruthy();
  });

  // get encourageAmount(), get encourageBonus() 테스트
  [
    { initialAmount: 12000, encourageAmount: 3000, encourageBonus: 500 },
    ......
  ].forEach(({ initialAmount, encourageAmount, encourageBonus }) => {
    it(`encourageAmount, encourageBonus 검증 (초기 금액: ${initialAmount}) => encourageAmount: ${encourageAmount}원 encourageBonus: ${encourageBonus}원`, () => {
      component.calculator.setAmount(initialAmount);
      fixture.detectChanges(); // ngOnChanges 실행 => 인위적으로 라이프사이클을 돌려준다

      const encourage = component.childComponent.encourage;
      expect(encourage.amount).toBe(encourageAmount);
      expect(encourage.bonus).toBe(encourageBonus);
    });
  });
});
```

