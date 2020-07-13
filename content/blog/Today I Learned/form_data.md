---
title: 'form data'
date: 2019-11-13 16:00:00
category: 'Today I Learned'
draft: false
---

## FormData

웹 애플리케이션 파일 업로드는 크게 두가지 방식이 있다.

- multipart/form-data

  FormData 객체를 사용하여 <input type="file"> 요소로부터 취득한 file 정보를 append하여 서버로 전송하는 방식이다.

- applecation/x-www-urlencoded

  클라이언트는 바이너리 파일을 Base64 인코딩하여 문자열화한 후, 서버로 전송하고 서버는 Base64 인코딩된 문자열을 디코딩하여 저장하는 방식이다. 인코딩으로 인한 성능 이슈가 발생할 수 있다.



# ISSUES

- [ ]  **`Error: mat-form-field must contain a MatFormFieldControl`**

⇒ This error occurs when you have not added a form field control to your form field. If your form field contains a native `` or `` element, make sure you've added the `matInput` directive to it and have imported `MatInputModule`. Other components that can act as a form field control include ``, ``, and any custom form field controls you've created.

```
@NgModule({
  imports: [
    **MatInputModule**
  ],
```

- [x]  `ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor`

  ```
    constructor() {
        super();
      }
  ```

- [ ]  `Can't resolve all parameters for Component: ([object Object], [object Object], [object Object], [object Object], [object Obj`

  private alertService: IMapiaAlertService

  //import from export interface IMapiaAlertService

  private alertService: MapiaAlertService

  //import from export declare class MapiaAlertService implements IMapiaAlertService

⇒ 에러가 해결 된 이유가 무엇일까???
