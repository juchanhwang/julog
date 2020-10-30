---
title: "Material Issue"
date: 2020-08-31 16:00:00
category: 'Angular'
draft: false
---

### ISSUE

```scss
::ng-deep .cdk-overlay-pane {
  height: 52px;
  padding: 0 12px;
  transform: translateY(50px);
}
```

<img width="680" alt="스크린샷 2020-08-31 오후 4 11 39" src="https://user-images.githubusercontent.com/36187948/91692676-bad66c80-eba4-11ea-93af-4b80b01e98c2.png">

snackbar를 띄울때 적용한 css코드로 인해 다른 <span style='color:#a6120d'>@angular/material</span> 에도 동일하게 값이 적용되는 문제가 발생함

기본으로 내장되어있는 class는 사용하지 않는 것을 권고

<br>

> snackBar의 MatSnackBarConfig 파라미터에 panelClass를 생성해서 style을 관리하면 된다

```ts
this.snackBar.openFromComponent(MobileErrorSnackbarComponent, {
    panelClass: `snackbar-container`,
  };
);
```

