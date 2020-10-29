---
title: '[Angular] form validator'
date: 2020-07-24 16:00:00
category: 'Angular'
draft: true
---



### 기본 validator

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

<br>

> Directive

```ts
@Directive()
export abstract class AbstractControlValueAccessorV2<T = any> extends AbstractControlValueAccessor<T> implements Validator {

  onValidatorChange: () => void = () => {};

  registerOnChange(fn: any): void {
    this.onChange = () => {
      fn();
      this.onValidatorChange();
    };
  }

  registerOnValidatorChange(fn: () => void): void {
    this.onValidatorChange = fn;
  }

  abstract validate(control: AbstractControl): ValidationErrors | null;
}
```



### custom validator

```ts
const address = new FormControl('', [ forbiddenNameValidator(/bob/i) ]);

function forbiddenNameValidator(nameRe: RegExp): ValidatorFn {
  return (control: AbstractControl): {[key: string]: any} | null => {
    const forbidden = nameRe.test(control.value);
    return forbidden ? {'forbiddenName': {value: control.value}} : null;
  };
}
```

