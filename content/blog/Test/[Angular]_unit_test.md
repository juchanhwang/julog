---
title: "[Angular] Unit Test"
date: 2020-09-28 16:00:00
category: 'Test'
draft: false
---

## 캐시 추가 충전 버튼 테스트하기

<img width="400" alt="스크린샷 2020-09-28 오후 11 06 46" src="https://user-images.githubusercontent.com/36187948/94442838-60e6b800-01df-11eb-9814-acdc510e62bd.png">

아래의 코드는 캐시 추가 충전 버튼에 대한 테스트 코드이다.

> krw-cash-calculator.spec.ts

```ts
describe('KRWCashCalculator', () => {
  let calculator: ICashCalculator;
  beforeEach(() => {
    calculator = new KRWCashCalculator();
  });

  // setAmount 함수 테스트
  [
    { initialAmount: 10000, amount: 10000 },
    { initialAmount: 100000, amount: 100000 },
  ].forEach(({ initialAmount, amount }) => {
    it(`setAmount 검증 (초기 금액: ${initialAmount}) => calculator.amount: ${amount}원`, () => {
      calculator.setAmount(initialAmount);
      expect(calculator.amount).toBe(amount);
    });
  });

  // plus 버튼 테스트
  [
    { initialAmount: 12000, nextAmount: 15000, nextBonus: 2000 },
    { initialAmount: 15000, nextAmount: 20000, nextBonus: 3000 },
    { initialAmount: 20000, nextAmount: 22000, nextBonus: 3000 },
    { initialAmount: 50000, nextAmount: 55000, nextBonus: 8000 },
    { initialAmount: 55000, nextAmount: 60000, nextBonus: 9000 },
    { initialAmount: 100000, nextAmount: 110000, nextBonus: 16500 },
    { initialAmount: 990000, nextAmount: 1000000, nextBonus: 150000 },
  ].forEach(({ initialAmount, nextAmount, nextBonus }) => {
    it(`plus 검증 (초기 금액: ${initialAmount}) => ${nextAmount}원  (+${nextBonus}원 적립)`, () => {
      calculator.setAmount(initialAmount);
      calculator.plus();
      expect(calculator.amount).toBe(nextAmount);
      expect(calculator.bonus).toBe(nextBonus);
    });
  });

  // minus 버튼 테스트
  [
    { initialAmount: 15000, prevAmount: 12000, prevBonus: 1500 },
    { initialAmount: 20000, prevAmount: 15000, prevBonus: 2000 },
    { initialAmount: 22000, prevAmount: 20000, prevBonus: 3000 },
    { initialAmount: 55000, prevAmount: 50000, prevBonus: 7500 },
    { initialAmount: 60000, prevAmount: 55000, prevBonus: 8000 },
    { initialAmount: 110000, prevAmount: 100000, prevBonus: 15000 },
    { initialAmount: 1000000, prevAmount: 990000, prevBonus: 148500 },
  ].forEach(({ initialAmount, prevAmount, prevBonus }) => {
    it(`minus 검증 (초기 금액: ${initialAmount}) => ${prevAmount}원  (+${prevBonus}원 적립)`, () => {
      calculator.setAmount(initialAmount);
      calculator.minus();
      expect(calculator.amount).toBe(prevAmount);
      expect(calculator.bonus).toBe(prevBonus);
    });
  });

  // get bonus() 테스트
  [
    { initialAmount: 12000, bonus: 1500 },
    { initialAmount: 15000, bonus: 2000 },
    { initialAmount: 20000, bonus: 3000 },
    { initialAmount: 55000, bonus: 8000 },
    { initialAmount: 95000, bonus: 14000 },
    { initialAmount: 1000000, bonus: 150000 },
  ].forEach(({ initialAmount, bonus }) => {
    it(`bonus 검증 (초기 금액: ${initialAmount}) => +${bonus}원 적립`, () => {
      calculator.setAmount(initialAmount);
      expect(calculator.bonus).toBe(bonus);
    });
  });
});
```

총 4개의 테스트를 작성해보았다. 충전금을 설정하는 **setAmount** 테스트, 충전금을 추가하기 위한 **plus** 테스트, 빼기 위한 **minus** 테스트, 그리고 충전금에 대한 **보너스(적립금)**에 대한 테스트이다. 

<br>

##### 테스트 코드를 작성해야하는 이유

- 모두를 위한 배려
  - 인간은 망각의 동물이다. 본인이 작성한 코드도 하루 이틀 지나면 헷갈리기 마련이다. 나도 기억하고 동료를 위해 필요하다.

- 코드 명세
  - 코드를 파악할 때 왜 이 코드를 짰는지 알 수 있는 안내판이 되기도한다고 생각한다. 이 함수가, 이 값이 반드시 가져야하는 결과값이 무엇인지 유추가 가능하다.
- 리팩토링할 때 유용하다.
  - 로직을 바꿔야하거나, 레거시를 제거해야할 때 중요한 코드는 건드리기 무섭다. '잘 되던 코드가 나 때문에 안 되면 어떡하지?', '이거 지워도 되는 코드인가. 분명히 이유가 있을 거 같은데..'. 리팩토링할 때 항상 드는 고민인 것 같다. 테스트 코드는 이러한 시행 착오를 줄여줄 수 있다고 본다.
- 테스트가 가능하다
  - TDD로 코드를 작성하게 되면, 반대로 어떻게 코드를 작성해야하는지 제약을 주기 때문에, 제한된 규칙 내에서 코드를 작성할 수 있다. 마치 타입을 정의하고 코드를 작성하는 것과 비슷한 논리라고 생각한다.