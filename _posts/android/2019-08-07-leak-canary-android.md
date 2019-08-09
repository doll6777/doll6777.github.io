---
layout: post
title: 안드로이드 LeakCanary를 사용한 힙덤프 분석
excerpt: "Android"
tags: [android, programming, thread, handler]
permalink: /android/:year/:month/:day/:title/
category : 안드로이드
---

> [LeakCanary Documentation](https://square.github.io/leakcanary/getting_started/) 을 보고 작성하였습니다.

## 메모리 릭이란
자바 기반의 런타임에서의 메모리 릭(누수)은 어플리케이션에서 더이상 필요로 하지 않는 객체의 참조를 가지고 있는 것이다.  
이렇게 되면 그 객체에 할당된 메모리는 다시 사용할 수 없고, 이는 OutOfMemoryError (OOM) 을 발생시킨다.  

### 메모리 릭이 발생하는 흔한 경우
주로 객체의 라이프사이클과 관련된 버그로 인해 발생한다.  
안드로이드에서의 흔한 실수는 다음과 같다.

- 액티비티의 컨텍스트를 필드로 저장하는 것. 설정(configration) 변경으로 인해 액티비티가 재생산 되는경우
- 객체의 라이프사이클을 가지고 있는 리스너, 브로드캐스트 리시버나 RxJava subscription 을 등록하고 라이프사이클이 끝났을 때 해제하는 것을 잊어버린 것.  
- 뷰를 정적(static) 필드에 저장하고 뷰가 해제(detach)될 때 필드를 지우지 않은것

## LeakCanary
Square사가 만든 오픈소스 라이브러리. 메모리 누수는 안드로이드 앱에서 흔하고 OOM은 크래시의 이유중 가장 빈번하다. 그러나 정확하게 카운팅되진 않는다. 만약 메모리가 적다면 OOM이 어느 곳에서나 발생할 수 있다. 

### LeakCanary가 동작하는 방식

#### 힙덤프
이런 객체들이 임계치를 넘기면 LeakCanary는 자바의 힙을 안드로이드 파일 시스템에 .hprof 파일로 저장한다. 
앱이 보일때의 기본 앱이 임계값은 5이고, 보이지 않을때의 임계값은 1이다.  

#### 덤프분석
LeakCanary는 .hprof 파일을 Shark를 사용해서 파싱하고 GC되지 않는 인스턴스의 참조의 체인을 찾는다. 
LeakTrace가 한번 정해지고 나면 LeakCanary는 안드로이드 프레임워크의 내장된 기능을 통해 어떤 인스턴스에서 메모리가 누수되고 있는지 찾는다.  

#### 누수 그루핑
누수 상태 정보를 통해서 LeakCanary는 참조 체인을 서브 체인으로 좁히고 결과를 출력해준다.  
평소의 체인과 비슷한 누수를 묶어 그루핑한다.   

### LeakCanary 사용법
단지 build.gradle에 추가만 하면 된다! 코드가 더이상 필요없다. LeakCanary는 디버그 빌드시 자동으로 메모리 릭이 일어났을 경우 노티를 준다.  

<pre class="prettyprint">
dependencies {
  // debugImplementation because LeakCanary should only run in debug builds.
  debugImplementation 'com.squareup.leakcanary:leakcanary-android:2.0-beta-2'
}
</pre>

### Shark 사용법

샤크는 LeakCanary2에 들어가는 힙 분석기이다. 100% 코틀린 라이브러리이다. 메모리를 줄이기 위해 sealed class에 의존한다.  

샤크는 
[Shark Download](https://github.com/square/leakcanary/releases/download/v2.0-beta-2/shark-cli-2.0-beta-2.zip) 를 통해 다운로드 가능하다.  

샤크는 다음과 같이 나뉘어져 있다.
1. Shark Hprof : hprof 파일을 읽고 쓴다
2. Shark Graph : 힙 오브젝트 그래프로 이동한다
3. Shark : 힙 분석 리포트를 만들어준다
4. Shark Android : 힙 분석 리포트를 잘 정제해서 안드로이드용으로 만들어 준다
5. Shark CLI : 안드로이드 디바이스에 설치된 힙을 분석한다. CLI를 사용할 경우엔 LeakCanary 디펜던시를 추가하지 않아도 된다.  
6. LeakCanary : 자동으로 더이상 사용하지 않는 액티비티나 프래그먼트를 감시하고 힙덤프를 트리깅한다. 

#### Shark CLI 사용법
샤크 CLI를 사용하면 실행중인 안드로이드 프로세스의 힙 정보들을 출력하고 분석할 수 있다. 
<pre class="prettyprint">
$ ./bin/shark-cli
</pre>

<pre class="prettyprint">
Commands: [analyze-process, dump-process, analyze-hprof, strip-hprof]
</pre>

hprof 파일을 분석하면 아래와 같은 결과를 볼 수 있다.  
<pre class="prettyprint">
    ┬
    ├─ leakcanary.internal.InternalLeakCanary
    │    Leaking: NO (it's a GC root and a class is never leaking)
    │    &darr; static InternalLeakCanary.application
    ├─ com.example.leakcanary.ExampleApplication
    │    Leaking: NO (Application is a singleton)
    │    &darr; ExampleApplication.leakedViews
    │                         ~~~~~~~~~~~
    ├─ java.util.ArrayList
    │    Leaking: UNKNOWN
    │    &darr; ArrayList.elementData
    │                ~~~~~~~~~~~
    ├─ java.lang.Object[]
    │    Leaking: UNKNOWN
    │    &darr; array Object[].[0]
    │                     ~~~
    ├─ android.widget.TextView
    │    Leaking: YES (View detached and has parent)
    │    View#mAttachInfo is null (view detached)
    │    View#mParent is set
    │    View.mWindowAttachCount=1
    │    &darr; TextView.mContext
    ╰&rarr; com.example.leakcanary.MainActivity
    ​     Leaking: YES (RefWatcher was watching this and MainActivity#mDestroyed
is true)
</pre>


#### Shark Code Example
> [How-to-Use-Shark](https://square.github.io/leakcanary/shark/) 에 코드가 잘 나와 있으니 참고하세요.


