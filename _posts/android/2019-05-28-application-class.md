---
layout: post
title: 안드로이드 Application Class
excerpt: "Android"
tags: [android, programming, thread, handler]
permalink: /android/:year/:month/:day/:title/
category : 안드로이드
---


# Android application class
컴포넌트들 사이에서 공동으로 멤버들을 사용할 수 있게 해주는 편리한 공유 클래스이다.  
어플리케이션 사이의 컴포넌트들이 공동으로 사용할 수 있기 때문에 공통되게 사용하는 내용을 작성하면 어디서든 context를 이용한 접근이 가능하다.  

1) Application Class 구현
<pre class="prettyprint">
class RanApplication : Application() {

    override fun onCreate() {
        super.onCreate()
        Log.i("ran", "[ran] RanApplication - onCreate()")
    }
}
</pre>

2) AndroidManifest.xml android:name에 등록
<pre class="prettyprint">
  <application
    android:allowBackup="true"
    android:icon="@drawable/ic_launcher"
    android:label="@string/app_name"
    android:name=".RanApplication"
    />
</pre>

3) Activity에서의 접근
<pre class="prettyprint">
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val context = applicationContext as RanApplication
        Log.i("ran", "this is text from application Context!" ${context.ranText})
    }
</pre>