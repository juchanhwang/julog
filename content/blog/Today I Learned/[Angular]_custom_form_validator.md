---
title: '[Angular] custom form validator'
date: 2020-07-24 16:00:00
category: 'Today I Learned'
draft: true
---



```ts
@Component({
  providers: [
    getNgValidators(MobilePositionFormComponent)
  ]
})

export class Formcomponent implements Validator {
  form = {
    address: new FormControl('', [ Validators.required ]),
    detailAddress: new FormControl(''),
  }
  
  formGroup: FormGroup = new FormGroup(this.form);
}
```

<br>

> Validator를 부모에게 전달하는 방법

```ts
validate(): ValidationErrors | null {
  const { controls } = this.formGroup;
  const errors = Object.keys(controls).reduce((acc, cur) => {
    return Object.assign(acc, controls[cur].errors);
  }, {});

  return Object.keys(errors).length > 0 ? errors : null;
}
```

<br>

> 간단하게 작성하기, 하나의 formControl만 검사

```ts
validate(): ValidationErrors | null {
  return  this.form.address.error;
}
```

