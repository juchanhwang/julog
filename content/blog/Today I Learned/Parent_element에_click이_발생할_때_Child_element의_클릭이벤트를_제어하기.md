---
title: "Parent element에 click이 발생할 때 child element의 클릭이벤트를 제어하기"
date: 2020-07-03 16:00:00
category: 'Today I Learned'
draft: false
---



## ISSUE

- `문의` 버튼을 클릭했을 때 모달이 열리는 이슈
- 아이템 전체 요소에 `click` event를 걸어서 자식 요소를 클릭했을 때도 event가 발생하는 원인

<video width="100%" height="300" controls>
  <source src="./sources/preventOpenModal.mov" type="video/mp4">
</video>



## 해결 방안

- click event를 인자로 넘겨준다.
- 문의 버튼 element를 ViewChild로 받아서 ts에서 제어한다.
- `telEl`element에 event.target이 포함(contains()) 되어있지 않으면 모달을 열어준다.

> HTML

```html
<div class="item-container" (click)="openModal($event)">
  ......
  <a #telEl [href]="'tel:'+lesson.phoneNumber[0]">
    <si icon="phone-alt-solid"></si>
    <span>문의</span>
  </a>
  ......
</div>
```

> TS

```ts
@ViewChild('telEl') telEl: ElementRef;

openModal(event) {
  if (!this.telEl.nativeElement.contains(event.target)) {
    this.bsModalService.show(MobileAcademyItemModalComponent, {
      initialState: { lesson: this.lesson }
    })
  }
}
```

