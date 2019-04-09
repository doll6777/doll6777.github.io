---
layout: post
title: Android Architectures (MVC, MVP, MVVM)
excerpt: "Android"
tags: [android, programming, architecture]
permalink: /android/:year/:month/:day/:title/
category : 안드로이드
---

# 안드로이드 아키텍쳐 패턴

### MVC (Model View Controller)

![mvc](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b5/ModelViewControllerDiagram2.svg/200px-ModelViewControllerDiagram2.svg.png)  
출처: 위키피디아

> *모델은 데이터 + 상태 + 비즈니스 로직이다.*   
> *뷰는 모델을 시각적으로 **표현**한 것이다.*   
> *컨트롤러는 사용자 입력에 반응하는 컴포넌트로, 인간과 앱을 잇는 다리 역할을 한다.*

안드로이드에서 MVC 패턴을 구현한다면 Controller는 Activity가 된다.


### MVP (Model View Presenter)
컨트롤러의 책임에 묶이지 않고도 뷰와 액티비티가 자연스럽게 결합하도록 한다.  
> *모델은 MVC와 동일하다.*  
> *프리젠터는 MVC의 컨트롤러와 같지만 뷰에 연결되지 않는다. 그냥 인터페이스다.*

Controller를 인터페이스로 만들어 Activity가 그 인터페이스를 구현하게 한다.  
Activity에 대한 종속성이 강한 안드로이드에서는 MVP 패턴이 깔끔하다.  

단위 테스트가 용이해진다.

### MVVM (Model View ViewModel)
1990년대 마틴 파울러에 의해 MVP에서 파생된 패턴이다.  Microsoft에 의해서 제안되었기 때문에 MSDN 문서를 기반으로 이해하는것이 좋다.  

MVVM의 핵심은 **Data Binding** 기술이다. 이 기술이 ViewModel이 View의 존재를 알지 못하게 하여 플랫폼 의존성에서 벗어나게 해준다.  

MVP에서 View-Presenter가 1:1 관계라면 MVVM에서는 1:n의 관계로 구성할 수 있다. 따라서  ViewModel을 여러 화면에서 재활용할수 있다.  


https://tomyrhymond.wordpress.com/2011/09/16/mvc-mvp-and-mvvm/
![pattern](https://tomyrhymond.files.wordpress.com/2011/09/mvc-mvp-mvvm.png)

### AAC (Android Architecture Component)
구글에서 새로운 라이브러리를 발표한 것이고, 안드로이드 앱을 개발하면서 자주 만날 수 있는 문제들을 쉽게 해결할 수 있게 하였다. 
LiveData, ViewModel, Room, Paging 등을 제공한다.