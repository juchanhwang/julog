---
title: 'bootsrap modal css 코드 ::ng-deep 으로 캡슐화하기'
date: 2019-12-12 16:00:00
category: 'Today I Learned'
draft: false
---

- bootsrap modal css 코드 `::ng-deep` 으로 캡슐화하기.

  ```html
  <div class="modal-dialog">
  	<div class="modal-content">
  		<mapia-cash-modal></mapia-cash-modal>
  	</div>
  </div>
  ```

  ```scss
  ::ng-deep .modal-dialog {
    width: 478px;
  
    > .modal-content {
      padding: 30px;
      border-radius: 10px;
    }
  }
  ```

  ⇒ `ISSUE` : 동일한 ngx-bootsrap modal 컴포넌트를 사용하는 컴포넌트에도 css 값이 먹힌다.

  - 다른 컴포넌트에 영향 주지 않으면서 컴포넌트 캡슐화 하기

  1. Set new class to modal window

  ```ts
  //current component.ts file
  
  constructor(public bsModalRef: BsModalRef)
  
  this.bsModalRef.setClass('modal-cash-plus');
  ```

  1. add css code

  ```scss
  ::ng-deep .modal-cash-plus {
    width: 478px;
  
    > div {
      padding: 30px;
      border-radius: 10px;
    }
  }
  ```

  - RESULT

  ```html
  <div class="modal-dialog modal-cash-plus">
  	<div class="modal-content">
  		<mapia-cash-modal></mapia-cash-modal>
  	</div>
  </div>
  ```

  ### (지원 중단) `/deep/`, `>>>`, `::ng-deep`

  컴포넌트 스타일은 보통 해당 컴포넌트의 템플릿에만 적용합니다.

  가상 클래스 `::ng-deep`가 적용된 CSS는 컴포넌트의 뷰 캡슐화 정책을 완전히 무시합니다. 그래서 `::ng-deep`이 적용된 규칙은 전역 스타일 규칙이 되기 때문에 해당 컴포넌트는 물론이고 이 컴포넌트의 자식 컴포넌트에 모두 적용됩니다. 그리고 `:[host]()` 셀렉터 앞에 `::ng-deep` 클래스를 사용하거나 `:[host]()` 셀렉터를 사용하지 않으면 해당 CSS 규칙은 다른 컴포넌트에도 모두 적용되니 주의해야 합니다.

  아래 예제는 컴포넌트 뷰 안에 있는 모든 자식 컴포넌트의 `` 엘리먼트에 이탤릭 속성을 지정하는 예제 코드입니다.

  > 참조: [앵귤러 공식 문서](https://angular.kr/guide/component-styles#지원-중단-deep--ng-deep)

  # The `::ng-deep` pseudo-class selector

  If we want our component styles to cascade to all child elements of a component, but not to any other element on the page, we can currently do so using by combining the `:host` with the `::ng-deep` selector:

  So this style will be applied to all `h2` elements inside `app-root`, but not outside of it as expected.

  This combination of selectors is useful for example for applying styles to elements that were passed to the template using `ng-content`, have a look at this [post](http://blog.angular-university.io/angular-ng-content/) for more details.

  > [Angular :host, :host-context, ::ng-deep - Angular View Encapsulation](https://blog.angular-university.io/angular-host-context/)
