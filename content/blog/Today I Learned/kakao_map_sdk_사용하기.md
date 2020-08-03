---
title: 'kakao map sdk 사용하기'
date: 2020-08-01 16:00:00
category: 'Today I Learned'
draft: false
---

### 초기 설정

> index.html 파일에 아래와 같이 추가하여 라이브러리를 로드한다.

```html
<script type="text/javascript" src="http://dapi.kakao.com/v2/maps/sdk.js?autoload=false"></script>
<script type="text/javascript">
kakao.maps.load(() => {
    // v3가 모두 로드된 후, 이 콜백 함수가 실행됩니다.
    var map = new kakao.maps.Map(node, options);
});
</script>
```

- autoload=false

  v3 스크립트를 동적으로 로드하기위해 사용한다.
  스크립트의 로딩이 끝나기 전에 v3의 객체에 접근하려고 하면 에러가 발생하기 때문에
  로딩이 끝나는 시점에 콜백을 통해 객체에 접근할 수 있도록 해준다.
  비동기 통신으로 페이지에 v3를 동적으로 삽입할 경우에 주로 사용된다.
  v3 로딩 스크립트 주소에 파라메터로 `autoload=false` 를 지정해 주어야 한다.

<br>

<br>

### 사용법

> kakao-map.component.ts

```html
<div #map
     [style.width]="styleOptions.width"
     [style.height]="styleOptions.height"
     [style.borderRaidus]="styleOptions.borderRadius">
</div>
<ng-content></ng-content>
```

```ts
export class KakaoMapComponent implements AfterViewInit {
  @Input() styleOptions: Partial<KakaoLibrary.StyleOptions>;
  @Input() coordinate: [ number, number ];
  @Input() level;
  
  @ViewChild('map') mapEl: ElementRef;

  ngAfterViewInit() {
    this.renderMap(
      this.mapEl.nativeElement,
      this.coordinate,
      this.level
    ).pipe(
      tap((map) => this.map$.next(map))
    ).subscribe();
  }
  
  private renderMap(
    mapContainer: HTMLElement,
    coordinate: [ number, number ],
    level: number
  ): Observable<KakaoLibrary.Map> {
    return new Observable((observer) => {
      kakao.maps.load(() => {
        const coords = new kakao.maps.LatLng(coordinate[0], coordinate[1]);
        const map = new kakao.maps.Map(mapContainer, {
          center: coords,
          level
        });

        window.setTimeout(() => {
          map.relayout();
          map.setCenter(coords);
          observer.next(map);
          observer.complete();
        });
      });
    });
  }
}
```

Angular가 구성 요소의 뷰와 자식 뷰를 초기화 한 후인, **ngAfterViewInit** 시점에 맵을 랜더링한다.

인자 값으로 맵 엘리먼트 요소, 좌표 값(위도, 경도), 그리고 맵의 zoom level 을 넘긴다.



> `map.relayout()`함수는 지도 영역 크기를 동적으로 변경할 때(모달 형태로 맵을 띄우거나) 호출하는 함수이다. 크기를 변경한 이후에는 반드시 relayout 함수를 호출하여 픽셀과 좌표정보를 새로 설정해주어야 한다.




<br>

> show-map.component.html

```html
<kakao-map [styleOptions]="styleOptions" [coordinate]="coordinate" [level]=level>
</kakao-map>
```



### 참고 및 출처

> - https://apis.map.kakao.com/

