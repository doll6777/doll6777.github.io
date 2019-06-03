---
layout: post
title: Kotlin - Anonymous class
excerpt: "kotlin"
tags: [kotlin, programming, language]
permalink: /kotlin/:year/:month/:day/:title/
category : Kotlin
---

# Anonymous class
익명 클래스란, 클래스의 이름 없이 정의하는 것을 말합니다.  
자바에서는 다음과 같이 익명 클래스를 정의하였는데요.  다음은 서비스를 바인딩 후 ServiceConnection이라는 인터페이스를  
구현하는 익명 클래스를 정의한 것입니다.


<pre class="prettyprint">
ServiceConnection conn = new ServiceConnection() {
public void onServiceConnected(ComponentName name,
IBinder service) {
}
public void onServiceDisconnected(ComponentName name) {
}
};
</pre>

코틀린에서는 익명 클래스를 다음과 같이 사용합니다. *object* 키위드를 앞에 붙이면 사용 가능합니다.
<pre class="prettyprint">
    var conn = object: ServiceConnection {
        override fun onServiceDisconnected(name: ComponentName?) {
            TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
        }

        override fun onServiceConnected(name: ComponentName?, service: IBinder?) {
            TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
        }
    }
</pre>
