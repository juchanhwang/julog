---
title: 'ESLINT-airbnb 코드스타일 적용하기'
date: 2019-2-1 16:21:13
category: 'development'
draft: false
---



## ESLint (?)

> Linting tools like ESLint allow developers to discover problems with their JavaScript code without executing it. The primary reason ESLint was created was to allow developers to create their own linting rules.
>
> 출처: <https://eslint.org/docs/about/>

쉽게 말하면 문제가 있는 패턴이나 코드를 찾기 위해 사용되는 정적 분석 툴이다. 



## ESLint 설치하기

```bash
$ cd <project-folder>
$ npm init -y

$ npm install -g eslint
```

```bash
$ eslint --init

? How would you like to configure ESLint? Use a popular style guide
? Which style guide do you want to follow? airbnb
? What format do you want your config file to be in? JavaScript
```



설치를 완료했다면, airbnb 스타일을 사용하기 위해 설치해야할 다른 패키지들이 있다.

```bash
$ npm info "eslint-config-airbnb@latest" peerDependencies
{ eslint: '^4.19.1 || ^5.3.0',
  'eslint-plugin-import': '^2.14.0',
  'eslint-plugin-jsx-a11y': '^6.1.1',
  'eslint-plugin-react': '^7.11.0' }
```



패키지가 설치되어있다면 다음 명령어를 실행한다.

```bash
$ yarn add eslint-config-airbnb
or
$ npm i eslint-config-airbnb
```



그리고, 폴더에 `.eslintrc.js` 파일을 생성한다.

```js
module.exports = {
  env: {
    browser: true,
    es6: true,
    node: true,
  },
  extends: ['airbnb'],
  plugins: ['react'],
  rules: {
     "react/prefer-stateless-function": 0,
     "react/jsx-filename-extension": 0,
     "react/jsx-one-expression-per-line": 0
   },
```



eslint aribnb 스타일 적용하는 것을 마쳤습니다. vscode 작업환경에서 

(`command` + `shift` + `p` ) ==> `ESLINT: Fix all auto-fixable Problems` 

![스크린샷 2019-07-23 오후 1 56 11](https://user-images.githubusercontent.com/36187948/61683926-6aaa9b00-ad52-11e9-8305-341c99484394.png)

위와 같이 수행하면, 쉽게 문제를 해결할 수 있다.



> 참조: <https://velog.io/@velopert/eslint-and-prettier-in-react>
