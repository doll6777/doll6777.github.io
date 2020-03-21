---
layout: post
title: 클린 아키텍쳐, Clean Architecture (인사이트)
excerpt: "git"
tags: [git]
permalink: /etc/:year/:month/:day/:title/
category : Book
---

![image](https://www.google.com/url?sa=i&url=http%3A%2F%2Fm.yes24.com%2FGoods%2FDetail%2F77283734&psig=AOvVaw1SHkhCV41nyRYEYz2Vz2If&ust=1581336925976000&source=images&cd=vfe&ved=0CAIQjRxqFwoTCMi9n6C5xOcCFQAAAAAdAAAAABAD)
클린 아키텍쳐, 소프트웨어 구조와 설계의 원칙  
로버트 C. 마틴 지음  

SA(소프트웨어 아키텍트)가 되기 위하여 알아야 할 기본 원칙들이 책에 나와있다. 다양한 시스템 종류와 변하는 최신 패러다임에도 불구하고 아키텍쳐의 규칙이란 일관적이고 보편적이다.  

## 패러다임
### 객체지향 프로그래밍
#### 의존성 역전
전형적인 호출 트리의 경우 main 함수가 고수준 함수를 호출하고, 고수준 함수는 중간 수죽 함수를, 중간 수준은 저수준 함수를 호출한다. 이러한 호출 트리에서의 소스 코드 의존성의 방향은 반드시 제어흐름을 따르게 된다.  
하지만 다형성이 끼어들면 특별한 일이 일어난다.  

소스 코드 사이에 인터페이스를 추가함으로써 방향을 역전시키는 것이다. 이런 접근법을 통해 OO 언어로 개발된 시스템을 다루는 아키텍트는 소스 코드 의존성 전부에 대해 방향을 결정할 수 있는 절대적인 권한을 갖는다.  

이 힘으로, 업구 규칙이 데이터베이스와 UI에 의존하는 대신에, 소스 코드 의존성을 반대로 배치하여 UI가 업무 규칙에 의존하도록 만들 수 있다.  

### 함수형 프로그래밍