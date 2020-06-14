---
title: 'DOM 프로퍼티(20191105)'
date: 2019-11-05 16:00:00
category: 'Today I Learned'
draft: false
---



> DOM 프로퍼티(Property)는 HTML 요소의 어트리뷰트(Attribute)와는 다른 것이다. 브라우저는 HTML 문서를 파싱하고 DOM 트리로 변환하여 메모리에 적재한다. 이때 HTML 요소는 DOM 노드 객체로, HTML 어트리뷰트는 DOM 노드 객체의 프로퍼티가 된다. DOM 프로퍼티는 DOM 노드 객체가 갖는 프로퍼티를 말하며, HTML 어트리뷰트는 HTML 요소가 갖는 어트리뷰트(속성)을 말한다. 아래의 ‘어트리뷰트 바인딩’에서 자세히 살펴볼 것이다.

- Templatedptj [FormControlName] = "example"을 등록하기 위해서는 example = new FormControl(); 의 방식으로 등록을 해야함

- example = new FormGroup (); // X

- form ⇒ 곡 업로드

- compareForm ⇒ 곡 수정

  ```html
  <ng-container *ngTemplateOutlet="formTemplate; context: {form: form, formGroup: formGroup, type: 'default'}">
  </ng-container>
  
  <ng-container *ngIf="currentUser?.authLevel >= 201 && compareForm">
    <ng-container *ngTemplateOutlet="formTemplate; context: {form: compareForm, formGroup: compareFormGroup, type: 'current'}">
    </ng-container>
  </ng-container>
  ```

  

