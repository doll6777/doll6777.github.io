---
layout: post
title: Kotlin Coroutine
excerpt: "kotlin"
tags: [kotlin, programming, language]
permalink: /kotlin/:year/:month/:day/:title/
category : Kotlin
---


> 본 포스팅은 https://kotlinlang.org/docs/reference/coroutines/basics.html#bridging-blocking-and-non-blocking-worlds 를 참고하여 번역하였습니다.

### 코루틴 기초

<pre class="prettyprint">
import kotlinx.coroutines.*

fun main() {
    GlobalScope.launch { // launch a new coroutine in background and continue
        delay(1000L) // non-blocking delay for 1 second (default time unit is ms)
        println("World!") // print after delay
    }
    println("Hello,") // main thread continues while coroutine is delayed
    Thread.sleep(2000L) // block main thread for 2 seconds to keep JVM alive
}
</pre>

이 코드를 실행하면, 다음과 같은 결과를 얻게 된다.

```
Hello,
World!
```

본질적으로, 코루틴은 가벼운 스레드이다. 코루틴은 빌더와 CoroutineScope라고 불리는 컨텍스트와 함께 실행된다. 위의 코드에서는 GlobalScope에서 코루틴이 실행되었는데, GlobalScope의 생명주기(lifetime)는 애플리케이션의 생명주기(lifetime)에 한정되어 있다.

> GlobalScope.launch는 thread{} 로, delay(...)는 Thread.sleep(...)로 대체해도 같은 결과를 얻는다.

### 블로킹과 넌블로킹 세계를 연결하기
첫번째 예제에서는 넌블로킹 delay(...) 와 블로킹 Thread.sleep(...) 를 섞어 사용하였다. 

<pre class="prettyprint">
mport kotlinx.coroutines.*

fun main() { 
    GlobalScope.launch { // launch a new coroutine in background and continue
        delay(1000L)
        println("World!")
    }
    println("Hello,") // main thread continues here immediately
    runBlocking {     // but this expression blocks the main thread
        delay(2000L)  // ... while we delay for 2 seconds to keep JVM alive
    } 
}
</pre>

위 코드의 결과는 같다. 하지만 이 코드는 오직 넌블로킹 delay만 사용한다. 메인 스레드는 runBlocking이 끝날 때까지 기다린다.

이 코드는 조금 더 이상적으로 작성할 수 있다. runBlocking을 main 함수에 래핑하는 것이다.

<pre class="prettyprint">
import kotlinx.coroutines.*

fun main() = runBlocking&ltUnit&gt { // start main coroutine
    GlobalScope.launch { // launch a new coroutine in background and continue
        delay(1000L)
        println("World!")
    }
    println("Hello,") // main coroutine continues here immediately
    delay(2000L)      // delaying for 2 seconds to keep JVM alive
}
</pre>

이때의 runBlocking은 상위레벨의 메인 코루틴을 시작하는 역할을 한다. 이때 Unit을 반환하는 이유는 코틀린에서의 main 함수가 Unit을 반환해야 하기 때문이다.

### job을 기다리기

다른 코루틴이 실행되는 동안 시간을 delay 하는것은 좋은 접근방법이 아니다. 명시적으로 백그라운드 job이 실행 완료될 때까지 (넌블로킹 방식으로) 기다려 주자.

<pre class="prettyprint">
val job = GlobalScope.launch { // launch a new coroutine and keep a reference to its Job
    delay(1000L)
    println("World!")
}
println("Hello,")
job.join() // wait until child coroutine completes
</pre>

결과는 동일하지만 메인 코루틴에 있는 코드가 백그라운드 job이 도는 시간에 묶여있지 않게 됐다. 훨씬 좋은 접근이다.

### 동시성 구조화
우리가 GlobalScope.launch를 사용할때 상위 레벨의 코루틴을 만들게 된다. 코루틴은 가볍지만, 실행되는 동안 메모리 리소스를 사용하게 된다. 만약 너무 많은 코루틴을 만들게 되어 OOM이 발생하거나, 새로 만들어진 코루틴의 참조를 잊어버린다면 어떻게 할 것인가? 코루틴들의 참조들은 수동으로 관리해야 하고 실행된 코루틴들을 join해야 할 것이다.  

좋은 솔루션은 이것이다. 우리는 동시성 구조화를 적용할 수 있다. GlobalScope에서 코루틴을 launch하는 대신에 일반 스레드를 만드는 것처럼 특정한 스코프 안에서 코루틴을 실행시킨다.  

예제를 보면 main 함수가 runBlocking을 사용하여 코루틴으로 바뀐 것을 확인할 수 있다.  
모든 코루틴 빌더, **runBlocking**, 같은 것들은 코루틴 스코프를 해당 코드 블럭에 추가한다.  
이제 명시적으로 join을 사용할 필요 없이 이 스코프에서 코루틴을 실행할 수 있다. 왜냐하면 해당하는 스코프에 있는 코루틴들이 모두 마칠 때까지 runBlocking이 실행 완료되지 않을 것이기 때문이다.

<pre class="prettyprint">
import kotlinx.coroutines.*

fun main() = runBlocking { // this: CoroutineScope
    launch { // launch a new coroutine in the scope of runBlocking
        delay(1000L)
        println("World!")
    }
    println("Hello,")
}
</pre>

### 스코프 빌더
다른 코루틴 빌더로 인해 만들어진 코루틴 스코프 말고 자신만의 스코프를 coroutineScope 빌더를 사용하여 만들수 있다. 이것은 코루틴 스코프를 만들고 다른 자식들의 실행이 다 끝날 때까지는 완료되지 않는다.
**runBlocking** 과 **coroutineScope** 의 가장 큰 차이점은 coroutineScope가 자식들이 모두 실행 완료되는것을 기다리는 동안에 현재 스레드를 블록하지 않는다는 것이다.

<pre class="prettyprint">
import kotlinx.coroutines.*

fun main() = runBlocking { // this: CoroutineScope
    launch { 
        delay(200L)
        println("Task from runBlocking")
    }
    
    coroutineScope { // Creates a coroutine scope
        launch {
            delay(500L) 
            println("Task from nested launch")
        }
    
        delay(100L)
        println("Task from coroutine scope") // This line will be printed before the nested launch
    }
    
    println("Coroutine scope is over") // This line is not printed until the nested launch completes
}
</pre>

### 리팩토링을 위해 추출하기
launch{...} 안에 있는 블록을 함수로 분리하고 싶다면 suspend 수정자를 사용하면 된다.  

<pre class="prettyprint">
import kotlinx.coroutines.*

fun main() = runBlocking {
    launch { doWorld() }
    println("Hello,")
}

// this is your first suspending function
suspend fun doWorld() {
    delay(1000L)
    println("World!")
}
</pre>

### 코루틴은 light-weight 하다!
<pre class="prettyprint">
import kotlinx.coroutines.*

fun main() = runBlocking {
    repeat(100_000) { // launch a lot of coroutines
        launch {
            delay(1000L)
            print(".")
        }
    }
}
</pre>
이 코드는 100kK 코루틴을 실행하고, 1초 뒤에 모든 코루틴이 . 를 찍는다. 이제, 스레드로 실행해 보자. (아마도 코드가 OOM을 발생시킬 것이다.)

### 글로벌 코루틴은 데몬 스레드와 같다
이는 데몬 스레드와 같기 때문에 프로세스를 살아 있는 상태로 계속 두어선 안된다.

<pre class="prettyprint">
GlobalScope.launch {
    repeat(1000) { i ->
            println("I'm sleeping $i ...")
        delay(500L)
    }
}
delay(1300L) // just quit after delay
</pre>
