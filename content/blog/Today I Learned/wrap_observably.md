---
title: "Wrap Observably"
date: 2020-07-08 16:00:00
category: 'Today I Learned'
draft: true
---



## ISSUE

- 카카오 맵 sdk 사용
- 카카오 맵에서 네이버 맵, 구글 맵으로 바로 바꿀 수 있도록 추상화를 하기
  - 카카오 맵메소드 추상화
  - 서비스로 파일 분리
  - 옵저버블한 상태로 변경



### 추상화하기 전 로직

> main.ts

```ts
navigator.geolocation.getCurrentPosition((result) => {
    const geocoder = new kakao.maps.services.Geocoder();
    geocoder.coord2RegionCode(result.coords.longitude, result.coords.latitude, (coordRes, status) => {
      if (status === kakao.maps.services.Status.OK) {
        this.api.getDistrictFromCode(coordRes[0].code).pipe(
          tap(([ city = '', district = '', town = '' ]) => {
            this.formGroup.patchValue({
              city, district, town,
              position: [ result.coords.latitude, result.coords.longitude ]
            });
          })
        ).subscribe();
      } else {
        console.error(status);
      }
    });
  },
);
```



### 추상화 후 로직

> main.ts

```ts
this.geolocation.getCurrentPosition().pipe(
		mergeMap((result) => {
      return this.geolocation.coord2RegionCode(result.longitude, result.latitude).pipe(
        mergeMap((regionCode: string) => {
          return this.api.getDistrictFromCode(regionCode).pipe(
            tap(([ city = '', district = '', town = '' ]) => {
              this.formGroup.patchValue({
                city, district, town,
                position: [ result.latitude, result.longitude ]
              });
            }),
          )
        })
      )
    }),
).subscribe()
```

- 서비스를 사용하는 쪽에서는 서비스 내부로직에서 무엇을 하는지 모름
- 내부 로직이 어떻게 생겼는지는 모르지만 필요한 파라미터만 넘겨주면 됨



> service.ts

```ts
coord2RegionCode(longitude: number, latitude: number) {
    return new Observable(observer => {
      kakao.maps.load(() => {
        const geocoder = new kakao.maps.services.Geocoder();
        geocoder.coord2RegionCode(longitude, latitude, (coordRes, status) => {
          if (status === kakao.maps.services.Status.OK) {
            observer.next(coordRes[0].code);
            observer.complete();
          } else {
            observer.error(status);
          }
        });
      });
    });
  }
```

- 서비스 내부 로직

