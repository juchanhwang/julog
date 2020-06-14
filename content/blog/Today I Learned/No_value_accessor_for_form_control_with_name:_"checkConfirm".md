---
title: 'No value accessor for form control with name: "checkConfirm"(20200320)'
date: 2020-03-20 16:00:00
category: 'Today I Learned'
draft: false
---



cashComponent에 paymentTermsDescription 자식 컴포넌트가 있고 자식 컴포넌트에 formControl을 정의해놓은 상태에서 나는 에러였음.

코드에서의 에러가 아니라, 부모의 spec파일에 paymentTermsDescription 컴포넌트를 선언하지 않아서 발생하는 에러

⇒ checkConfirm이라는 form control에 value accessor가 없는 상태.

⇒ 자식에는 value accessor를 extends 했지만, 부모에는 없는상태. (부모에 똑같이 extends해도 에러는 그대로 있었음 why?)

```html
<mp-payment-terms-description [formControl]="checkConfirm"></mp-payment-terms-description>
```

