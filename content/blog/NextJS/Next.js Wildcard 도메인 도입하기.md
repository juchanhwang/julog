---
title: "Next.js Wildcard 도메인 도입하기"
date: 2024-10-07 18:00:00
category: 'NextJS'
draft: false
---

## 문제 배경

DT 진단을 할 수 있는 제품의 MVP를 만들고 있었다. MVP를 개발할 당시까지만 해도(합의된) 프로덕트의 도메인은 [dtlab.co.kr](http://dtlab.co.kr) 하나였다.

하지만, B2B 서비스여서 그런지 기업들의 요구사항은 제각각 이었다. 많은 기업에서 커스텀 도메인이 가능하도록 해달라는 요구사항이 있었다. MVP를 설계할 당시는 도메인에 대한 고민이 없었기에, 도메인에 대한 기업의 요구사항이 생기면 프로젝트를 새로 생성했다. 그렇게 기업의 개수는 늘어났고 5개가 넘어가자, 고통받는 것은 결국 나였다.

<img src="./images/Next.js Wildcard 도메인 도입하기-1.png" alt="Next.js Wildcard 도메인" width="300"/>

우리 서비스를 사용하는 고객사가 추가될 때마다 새로운 프로젝트를 생성해야헸고 동일한 코드의 프로젝트가 동일하게 늘어났다. 기능이 추가되거나 수정되면 모든 프로젝트에 복제 해야했으며, 배포시 각 프로젝트별 빌드가 되어야하기 때문에 배포 시간이 늘어났다. 현재 상황만 봐도 복잡한데 새로운 문제가 발생했다. 그것은 바로 고객사의 커스텀 기능이었다. 이 때부터는 감당이 되지 않는 수준으로 관리포인트가 생겼다.

## 문제 정의

- **문제 상황 1. 고객사별 커스텀 기능**
- **문제 상황 2. 고객사별 커스텀 도메인**

## **문제 해결 방법**

### **문제 상황 1. 고객사별 커스텀 기능**

- 커스텀이 가능한 영역과 불가능한 영역을 나누기
- 커스텀이 가능한 영역을 클라이언트가 아닌 서버에서 관리하기
- 서버에서 기관 코드 값을 내려주어, 클라이언트에서 사용

### **문제 상황 2. 고객사별 커스텀 도메인**

- 프로젝트를 하나로 통합
- 와일드카드 도메인 도입
- middleware에서 path redirect 및 rewrite

자 하나씩 해결해보자. 먼저, 프로젝트를 하나로  통합해보자. 프로젝트의 구조는 이러하다. 기존에 각 기관별로 존재했던 프로젝트가, 하나의 프로젝트 안에 통합이 되었다. 기관의 고유 코드 값을 기준으로 static하게 만들어준고, 기관별 페이지는 빌드시 생성이 되도록 했다.

<img src="./images/Next.js Wildcard 도메인 도입하기-2.png" alt="Next.js Wildcard 도메인" width="400"/>

기관별로 페이지를 생성했으니, 기관별 도메인이 필요하다. 와일드카드 도메인 등록하는 방법은 생각보다 어렵지 않은 작업이었다.

- https://vercel.com/blog/wildcard-domains

Vercel 공식 문서에 작성 되어있는 대로 도메인에 와일드 카드로 추가 하면 된다.

<img src="./images/Next.js Wildcard 도메인 도입하기-3.png" alt="Next.js Wildcard 도메인" width="700"/>

이제 각 페이지마다, 와일드 카드에 맞는 path로 이동하기 위해서는 Next.js의 middleware에서의 redirect와 rewrite가 필요하다.

- rewrite

  > Rewrites allow you to send users to different URLs without modifying the visible URL. They allow you to change the URL path, query parameters, and headers of the request before it reaches your server.
  >
    - 리라이트(Rewrites)는 사용자가 보게 되는 URL을 변경하지 않고, 다른 URL로 요청을 보내는 것을 뜻한다.
- redirect
    - **리다이렉트**(**Redirect**)는 사용자가 처음 요청한 URL이 아닌, 다른 URL로 보내는 것을 뜻한다.

먼저, 현재 주소로부터 어떤 기관인지 식별해주는 api를 호출한다.

```tsx
export const middleware = async (request: NextRequest) => {
  const url = request.nextUrl.clone();
  const path = url.pathname;
  const host = request.headers.get('host') || '';

  const organization = await getOrganizationCodeByUrl(host);
  if (!organization) {
    return NextResponse.next();
  }

  const { code } = organization;
  
  ......
};

```

코드가 식별된 후에, 최상위 path인 `/organizationCode` 와 일치하는지 비교한다. path와 일치하면, path를 제거한 주소로 redirect한다.

```tsx
  const isCorrectPath = path.startsWith(`/${code}`);

  if (isCorrectPath) {
    url.pathname = path.replace(`/${code}`, '');
    return NextResponse.redirect(url);
  }
 
```

예를들어, 현재 `org1.dtlab.co.kr/org1`에 접속하면 내부적으로 코드를 뺀 `org1.dtlab.co.kr`로 리다이렉트 합니다. 즉, `/org1`와같이 path를 노출하지 않기 위해서, path를 제거하는 redirect 코드를 삽입하였습니다.

반대로, 코드가 일치하지 않으면 코드를 추가하여 rewrite한다.

```tsx

  const isCorrectPath = path.startsWith(`/${code}`);
  
  if (!isCorrectPath) {
    url.pathname = `/${code}${path}`;
    return NextResponse.rewrite(url);
  }
```

예를들어, `org1.dtlab.co.kr`로 리다이렉트 후에 원래 정상적으로 동작하는 주소인 `org1.dtlab.co.kr/org1`로 rewrite합니다. 결과적으로 path에서는 기관 코드가 숨겨지는 효과를 얻을 수 있다.

### **최종 시나리오**

1.

`org1.dtlab.co.kr/sign-up` → (rewrite) →  `org1.dtlab.co.kr/org1/sign-up`

2.

`org1.dtlab.co.kr/org1/sign-up` → (redirect) →  `org1.dtlab.co.kr/sign-up` → (rewrite) → `org1.dtlab.co.kr/org1/sign-up`

### 최종 코드 결과물

- `root > src > middleware.ts`

```tsx
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

import { getOrganizationCodeByUrl } from './apollo/getOrganizationCodeByUrl';

const IMAGE_EXTENSIONS = [
  '.png',
  '.jpg',
  '.jpeg',
  '.gif',
  '.webp',
  '.svg',
  '.ico',
];

export const middleware = async (request: NextRequest) => {
  const url = request.nextUrl.clone();
  const path = url.pathname;
  const host = request.headers.get('host') || '';

  const organization = await getOrganizationCodeByUrl(host);
  if (!organization) {
    return NextResponse.next();
  }

  const { code } = organization;

  const isImageRequest = IMAGE_EXTENSIONS.some((ext) => path.endsWith(ext));
  const isCorrectPath = path.startsWith(`/${code}`);

  if (isCorrectPath) {
    url.pathname = path.replace(`/${code}`, '');
    return NextResponse.redirect(url);
  }

  if (!isImageRequest && !isCorrectPath) {
    url.pathname = `/${code}${path}`;
    return NextResponse.rewrite(url);
  }

  return NextResponse.next();
};

export const config = {
  matcher: [
    /*
     * Match all request paths except for the ones starting with:
     * - api (API routes)
     * - _next/static (static files)
     * - _next/image (image optimization files)
     * - favicon.ico (favicon file)
     */
    '/((?!api|_next/static|_next/image|favicon.ico).*)',
  ],
};

```

### 최종 결과 화면

<img src="./images/Next.js Wildcard 도메인 도입하기-4.png" alt="Next.js Wildcard 도메인" width="998"/>

<img src="./images/Next.js Wildcard 도메인 도입하기-5.png" alt="Next.js Wildcard 도메인" width="998" style="margin-top: 16px"/>

## Reference

- https://vercel.com/blog/wildcard-domains
- https://vercel.com/docs/edge-network/rewrites
- https://nextjs.org/docs/app/building-your-application/routing/middleware

