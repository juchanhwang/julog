---
title: 'ng-template & ng-container'
date: 2019-11-06 16:00:00
category: 'Angular'
draft: false
---

## ng-template

```html
<div *ngIf="true"> Hi </div>
```

위의 코드는 Angular에서 *ngIf를 처리할 때 아래와 같이 변환 됩니다.

```html
<ng-template [ngIf]=”true”>
    <div> Hi </div>
</ng-template>
```

ng-template은 기본적으로 **view에 랜더링 되지 않는다**. 단순히 보이지 않는게 아니라 DOM에서도 존재하지 않는다. 즉, ng-template을 사용하면 주석처리가 된다.

하지만, ng-template을 view에 보여주기 위해 두 가지 방법이 있다. **ngIf 구조 디렉티브를 사용하거나 템플릿 참조 변수**를 사용하는 것이다.

```html
<! — ngIf 사용 →>

<ng-template [ngIf]=”true”> ngif </ng-template>

<br>

<! — 템플릿 참조 변수 사용 →>
<div *ngIf=”true; then templateReferenceVariable”></div>

<ng-template #templateReferenceVariable>
template reference variable
</ng-template>
```

위 코드의 결과는 아래와 같으며 ng-template가 view에 랜더링 될 수 있는 조건(구조 디렉티브, 템플릿 참조변수)이 추가되면 ng-template과 관련된 주석이 추가된다.

**ng-template을 이용해서 view에 랜더링의 여부를 결정해야 한다면 템플릿 참조 변수를 사용하는 것이 좋다.**

## ng-contianer

ng-container는 Angular 그룹 요소로 ng-template과 두 가지 차이점이 있습니다. **ng-container는 항상 view에 랜더링 된다.**

```html
<ng-container *ngIf="true" > Hi </ng-container>
```

ng-container에서는 *ngIf로 표현할 수 있다. ng-container는 언제 사용하는 것일까?

```typescript
// app.component.html
<div *ngIf=”isClicked”> 버튼이 눌림!! </div> // (*)
<button (click)=”changeStatus()”>button </button>

// app.component.ts
export class AppComponent {
   isClicked = false;
   changeStatus() {
      this.isClicked = !this.isClicked;
   }
}
```

위 코드는 html의 버튼을 클릭하면 component 클래스의 isClicked 변수를 변경해서 `"버튼이 눌림!!"` 문자열을 보여주는 코드이다. 실제로 이런 용도로 수많은 div 태그가 존재하고 div 태그에 css를 적용하기 시작했다. 이럴 경우 DOM이 매우 복잡해져 디버깅이 어려워지고 문제가 발생하는 경우 쉽게 원인을 찾기 힘들다.

Angular에서는 ng-container를 쓴는게 더 좋은 방법이다. **Angular는 ng-container를 DOM에 넣지 않기 때문에 css나 layout에 영향을 미치지 않는다.**

*ngIf와 *ngFor을 사용할 때 좋은 예

```html
<div *ngFor=”let item of items”>
   <div *ngIf=”item.id”>
      {{item.name}}
   </div>
</div>>
```

⇒ ng-container를 사용했을 때

```html
<ng-container *ngFor="let item of items">
   <div *ngIf="item.id">
      {{item.name}}
   </div>
</ng-container>
```

## ngTamplateOutlet

ng-container에서 ngTemplateOutlet 구조 디렉티브를 이용해서 다음과 같이 사용할 수 있다.

```html
<ng-container *ngTemplateOutlet=”template”></ng-container> 
<ng-template #template> Hello!</ng-template>
```

ngTemplateOutlet 디렉티브를 이용하면 동일한 템플릿을 중복해서 사용할 수 있다. 가령 회사 로고 이미지를 템플릿 여러곳에서 보여준다고 할 경우 아래와 같이 사용할 수 있다.

```html
<div>
	여기는 header입니다.
	<ng-container *ngTemplateOutlet="logo"></ng-contaier>
</div>

<div>
	여기는 본문입니다.
	<ng-container *ngTemplateOutlet="logo"></ng-container>
</div>

<div>
	여기는 footer입니다.
	<ng-container *ngTemplateOutlet="logo"></ng-container>
</div>

<ng-template #logo>
	<div> <img [src]="logoImage"> </div>
</ng-template>
```
