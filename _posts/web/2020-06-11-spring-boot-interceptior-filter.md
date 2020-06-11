---
layout: post
title: Spring의 Interceptor, Filter, AOP의 차이
excerpt: "web"
tags: [programming, web]
permalink: /web/:year/:month/:day/:title/
category : web
---

요청이 들어오면 거치는 순서
```
Servlet Request => Filter => DispatcherServlet => Interceptor => APO => Controller
```

- 서버를 실행한 후 서블릿이 올라오는 동안에 init 실행, 그 후 doFilter 실행
- 컨트롤러에 들어가기 전 preHandler 실행

### Filter
요청과 응답을 필터링함. 보통 web.xml에 등록하고 XSS 방어 등에도 사용된다.  
스프링 컨텍스트 외부에 존재한다. 
- init()
- doFilter()
- destroy()

### Interceptor
요청에 대한 전/후로 가로챈다. 스프링 컨텍스트 내부에서 컨트롤러에 대한 요청과 응답에 대해 처리한다. 인터셉터는 여러개 사용가능하다.  
- preHandler()
- postHandler()
- afterCompletion()

### AOP
객체 지향 프로그래밍에서는 중복코드가 여러개 존재하게 되는 상황이 있다. 이것을 해결하기 위해 나온 개념이다.  
주로 로깅이나 트랜잭션, 에러처리가 필요할 때 사용한다.  
AOP는 메소드 전후의 지점에 설정할수 있다. 
- @Before
- @After
- @After-returning
- @After-throwing
- @Around