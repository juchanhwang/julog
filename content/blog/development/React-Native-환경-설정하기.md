---
title: 'React-Native 환경 설정하기'
date: 2019-07-20
category: 'development'
draft: false
---

IT 동아리 NEXTERS에서 앱을 만들고 있다.
나는 웹 프론트엔드 개발자이고, 안드로이드 개발자가 있다. 팀 회의 끝에 웹 개발자와 앱 개발자가 함께 개발 할 수 있는 환경을 위해 react-native를 사용하기로 결정했다.

리액트를 조금 다루어보았지만 RN은 처음이었다.
React-Native 공식 Document를 읽으며 환경 설정을 하였지만, 마주한 것은 에러였다.

![스크린샷 2019-07-20 오후 7.56.32](https://raw.githubusercontent.com/juchanhwang/julog/master/content/blog/development/images/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-07-20%20%EC%98%A4%ED%9B%84%207.56.32.png)

구글링을 하면서 문제를 해결해보려고 했지만 쉽지가 않았다.

ios 폴더에 build 폴더를 삭제하고 다시 실행해보기도 했었고, Xcode의 project setting도 바꿔보기도 했었다.

하지만, 가장 보편적인 방법이 해결해주지는 못했다.

#### 해결책

- CocoaPod Install

![스크린샷 2019-07-20 오후 7.57.19](https://raw.githubusercontent.com/juchanhwang/julog/master/content/blog/development/images/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-07-20%20%EC%98%A4%ED%9B%84%207.57.19.png)

- pod update

![스크린샷 2019-07-20 오후 7.57.39](https://raw.githubusercontent.com/juchanhwang/julog/master/content/blog/development/images/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-07-20%20%EC%98%A4%ED%9B%84%207.57.39.png)

설렘과 기대로 다시 실행 `react-native run-ios`

![스크린샷 2019-07-20 오후 7.58.16](https://raw.githubusercontent.com/juchanhwang/julog/master/content/blog/development/images/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202019-07-20%20%EC%98%A4%ED%9B%84%207.58.16.png)

**코코아팟이란?**

> "CocoaPods는 Swift 및 Objective-C 코코아 프로젝트의 종속성 관리자입니다.
>
> 28,000 개가 넘는 라이브러리를 가지고 있으며
>
> 170 만 개가 넘는 응용 프로그램(앱)에서 사용되고 있습니다.
>
> CocoaPod은 프로젝트를 **우아하게** 확장 할 수 있도록 도와줍니다."
>
> (출처: https://zeddios.tistory.com/25)

### 느낀점

> - 세상에 해결 할 수 없는 문제는 없는 것 같다. 그 문제를 해결하기 위해 내가 얼마나 많은 삽질(?)을 했는지가 중요하다. 이러한 과정들이 있어야 삽질(?)의 노하우가 생기고, 문제 해결능력 또한 향상되는 것 같다.
>
> - 환경설정은 언제나 어렵다 (왜 나한테만 이런 에러가 나는 것일까)
