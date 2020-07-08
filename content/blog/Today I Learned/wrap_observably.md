---
title: "Wrap Observably"
date: 2020-07-08 16:00:00
category: 'Today I Learned'
draft: true
---



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

