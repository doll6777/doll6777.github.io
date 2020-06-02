---
layout: post
title: CSS position 기본
excerpt: "web"
tags: [programming, web]
permalink: /web/:year/:month/:day/:title/
category : web
---

### static
기본 상태. 차례대로 왼->오, 위->아래로 쌓인다.  

### relative
static에서 태그의 위치를 변경하고 싶을때 top, right, bottom left 값을 설정하면 해당 픽셀만큼 이동한다.

### absolute
static 속성을 가지고 있지 않은 부모를 기준으로 움직인다. 만약 부모중 static이 없다면 가장 위의 태그가 기준이 된다.  

### fixed
스크롤을 움직이더라도 특정 위치에 고정되어 있다.  
