---
title: "We detected that the Chromium Renderer process just crashed GITLAB CI/CD 에러 해결하기"
date: 2020-04-26 16:00:00
category: 'Today I Learned'
draft: false
---



**ISSUE:**

`We detected that the Chromium Renderer process just crashed. This is the equivalent to seeing the 'sad face' when Chrome dies. This can happen for a number of different reasons:`

- `You wrote an endless loop and you must fix your own code`
- `There is a memory leak in Cypress (unlikely but possible)`
- `You are running Docker (there is an easy fix for this: see link below)`
- `You are running lots of tests on a memory intense application`
- `You are running in a memory starved VM environment`
- `There are problems with your GPU / GPU drivers`
- `There are browser bugs in Chromium`

**해결 방법**

- cypress/plugins/index.js 파일 안에 코드 추가

```ts
module.exports = on => {
// eslint-disable-next-line @typescript-eslint/explicit-function-return-type
  on('file:preprocessor', cypressTypeScriptPreprocessor);

  //Docker crashing issue when renderer process eats up too much memory
  on('before:browser:launch', (browser = {}, args) => {
    if (browser.name === 'chrome') {
      args.push('--disable-dev-shm-usage');
    }

    return args
  });
};
```

- gitlaba-ci-mapianist.yml 파일에 cypress docker image browser 버젼 업그레이드

```yml
test:pc:
  image: cypress/base:13.6.0
```

- GITLAB-RUNNER

  <img width="900" alt="_2020-02-18__11 39 43" src="https://user-images.githubusercontent.com/36187948/82394154-36dbf700-9a83-11ea-8aea-80f2a17b5fdc.png">

참고자료

> 참고자료:

- [[Pass --ipc=host option\]](https://gitlab.com/gitlab-org/gitlab-runner/issues/2168)
- [[Using Cypress in GitLab CI\]](https://lcx.wien/blog/cypress-gitlab-ci/)
