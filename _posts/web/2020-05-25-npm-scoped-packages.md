---
layout: post
title: NPM package 이름 앞에 붙는 골뱅이의 의미 - Scoped Package
excerpt: "web"
tags: [programming, web]
permalink: /os/:year/:month/:day/:title/
category : web
---

> npm help scope

npm package.json에서 보면 `@testing-library/jest-dom` 과 같이 골뱅이 기호가 붙은것을 볼 수 있다.  
이것이 일반 `jest`왐 무엇이 다른지에 대해 알아봤다.  

이것은 **scoped packages** 라고 불리는 NPM에 새로 추가된 기능이다.  
NPM 패키지에 네임스페잊스를 적용하는 것이다. 

- Global Modules : 현재 존재하는 컨벤션 네임을 따른 모듈이다.
- Scoped Modules : 조직이나 그룹하에 "Scoped" 된 새로운 모듈이다. `@조직이름/패키지이름` 으로 설치할 수 있다.  


참고한 페이지
- https://stackoverflow.com/questions/36667258/what-is-the-meaning-of-the-at-prefix-on-npm-packages
- https://docs.npmjs.com/misc/scope