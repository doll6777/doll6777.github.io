---
layout: post
title: Kotlin Coroutine Suspend
excerpt: "kotlin"
tags: [kotlin, programming, language]
permalink: /kotlin/:year/:month/:day/:title/
category : Kotlin
---

> 참고한 페이지 - https://sourcediving.com/kotlin-coroutines-in-android-e2d5bb02c275

# Suspend Function
suspend 수정자만 붙여주면 해당 함수가 코루틴의 실행을 중지할 수 있다.  
서스펜트 함수는 어떤 일반 함수라도 부르는 것이 가능하지만, 실제로 실행을 중지하기 위해서는 다른 서스펜드 함수여야 한다.  

반면에, 서스펜딩 함수는 일반 함수에서 불릴 수 없다. 그러므로 코루틴 빌더가 제공되고 이것은 일반 서스펜딩 스코프가 아닌 곳에서 서스펜딩을 부를 수 있게 해준다.  

코틀린 코루틴 라이브러리는 다음의 코루틴 빌더들을 제공한다.  

- runBlocking : 새로운 코루틴을 만들고 현재 스레드가 끝날 때까지 블록한다.
- launch : 새로운 코루틴을 만들고 레퍼런스를 Job라는 오브젝트로 리턴한다.
- async : 새로운 코루틴을 만들고 레퍼런스를 Deferred<T> 라는 오브젝트로 만든다. 이는 값을 리턴할 수 있다. async는 await 서스펜딩 함수와 함께 사용되어야 한다.

# Coroutine Dispatcher
안드로이드에서 UI관련 변경을 할때는 항상 메인 스레이드에서 해야하지 때문에, 코루틴에서는  
실행할 스레드를 선택할 수 있는것이 코루틴 빌더에 제공된다. 이를 Coroutine Dispatcher라고 한다.  

# Coroutine Cancel
백그라운드 태스크를 중지해야 할 때가 있을것이다. 예를들면 안드로이드에서 액티비티나 프래그먼트의 라이프 사이클이 끝났을때 말이다. 중지하지 않으면 메모리 릭이나 크래시로 이어질 수 있다.  

이를 해결하기 위하여 단순하게 cancel을 호출한다. job object를 코루틴 빌더에서 받은 후 cancel하면 되는데, 만약 취소되는 과정이 완료될 때까지 기다려야 하는 경우에는 cancelAndJoin 메소드를 부르면 된다.  

<pre class="prettyprint">
launch(UI) {
    job.cancelAndJoin()
    textView.text = "Cancelled"
}
</pre>