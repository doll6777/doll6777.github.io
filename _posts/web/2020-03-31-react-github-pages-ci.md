---
layout: post
title: React 페이지를 Github Pages 사용하여 배포하기
excerpt: "web"
tags: [programming, web]
permalink: /os/:year/:month/:day/:title/
category : Web
---

### Github Pages 만들기
- github인경우 : [username].github.io
- enterprise github인경우 : [username].[enterprise domain]
의 이름으로 레포지토리를 생성한다.  

### Settings 페이지 확인
Setting 페이지를 확인하면 어떤 url로 Publish 되었는지 확인할 수 있다.  
이때 레포지토리에 index.html이 없다면 해당  
url이 존재하지 않을 수 있다.  

### 리액트 배포
원래 작업하던 react project에서 yarn install gh-pages를 입력하여 package.json에 생기도록 한다.  

<pre class= "prettyprint">
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "deploy": "gh-pages -d build -b master"
  },
</pre>

deploy에 gh-pages를 부르는 스크립트를 추가한다.  
-d build : 빌드 폴더에 있는것들을
-b master : 마스터 폴더에 올린다

### yarn install && yarn deploy
<pre>
yarn deploy -r [page repository url]
</pre>

### 배포된 페이지 확인
settings에서 확인했던 페이지 url에 들어가 제대로 배포되었는지 확인한다.  

만약 파일이 잘 들어갔다면 page repository에 master 브랜치에 파일들이 들어가 있을 것이다.  

만약 master branch 말고 gh-pages와 같은 브랜치에 업로드 하고 싶다면, 세팅에서 보여지는 기본 브랜치를 바꾸고  
-b 설정을 없애거나 (default: gh-pages) 바꾸고 싶은 브랜치 이름을 쓰면 된다.  

