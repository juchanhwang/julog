---
title: 'EventEmitter'
date: 2019-11-08 16:00:00
category: 'Today I Learned'
draft: false
---



앵귤러에는 부모로 이벤트를 보내는 방법에 **@Output**과 **EventEmitter**가 있다.

- appchild.component.ts

  ```typescript
  import { Component, Input, EventEmitter, Output } from '@angular/core';
  @Component({
      selector: 'app-child',
      template: `<button class='btn btn-primary' (click)="handleclick()">Click me</button> `
  })
  export class AppChildComponent {
      handleclick() {
          console.log('hey I am  clicked in child');
      }
  }
  ```

  - AppChildComponent에는 handleclick 함수를 호출하는 button이 있다.

  

- appcomponent.ts

  ```typescript
  import { Component, OnInit } from '@angular/core';
  @Component({
      selector: 'app-root',
      template: `<app-child></app-child>`
  })
  export class AppComponent implements OnInit {
      ngOnInit() {
      }
  }
  ```

  - AppComponent안에 AppChildComponent를 사용하고 있다.
  - 만약에 AppChildComponent의 ts 코드 안에서 이벤트를 부모로 넘기고 싶을 때, **EventEmitter**와 **Output**을 `@angular/core`에서 import하면 된다.

  

- appchildcomponent.ts

  ```typescript
  import { Component, EventEmitter, Output } from '@angular/core';
  @Component({
      selector: 'app-child',
      template: `<button class='btn btn-primary' (click)="valueChanged()">Click me</button> `
  })
  export class AppChildComponent {
      @Output() valueChange = new EventEmitter();
      Counter = 0;
      valueChanged() { // You can give any function name
          this.counter = this.counter + 1;
          this.valueChange.emit(this.counter);
      }
  }
  ```

  

  ![https://www.infragistics.com/community/resized-image/__size/640x480/__key/communityserver-blogs-components-weblogfiles/00-00-00-09-43/3051.pic7.png](https://www.infragistics.com/community/resized-image/__size/640x480/__key/communityserver-blogs-components-weblogfiles/00-00-00-09-43/3051.pic7.png)
