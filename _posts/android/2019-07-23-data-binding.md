---
layout: post
title: 안드로이드 DataBinding (데이터 바인딩)
excerpt: "Android"
tags: [android, programming, thread, handler]
permalink: /android/:year/:month/:day/:title/
category : 안드로이드
---

## 데이터 바인딩이란?

> Part of Android Jetpack  
> https://developer.android.com/topic/libraries/data-binding  를 참고하여 작성하였습니다.

아래의 코드는 안드로이드 UI 프로그래밍시 많이 보는 코드이다.
<pre class="prettyprint">
findViewById&lt;TextView&gt;(R.id.sample_text).apply {
    text = viewModel.userName
}
</pre>

데이터 바인딩을 사용하면 위의 자바 코드를 부르는것을 생략 가능하다.
<pre class="prettyprint">
&lt;TextView
    android:text=&quot;@{viewmodel.userName}&quot; /&gt;
</pre>

## 데이터 바인딩 사용하기 

### 환경설정
build.gradle에 dataBinding enabled로 설정하기
<pre class="prettyprint">
android {
    ...
    dataBinding {
        enabled = true
    }
}
</pre>

### 레이아웃 파일
데이터 바인딩 레이아웃 파일은 일반 레이아웃 파일과 약간 다르다. root 태그를 layout으로 감싸고 그 다음 data와 view 엘리먼트가 따른다.
<pre class="prettyprint">
&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;
&lt;layout xmlns:android=&quot;http://schemas.android.com/apk/res/android&quot;&gt;
   &lt;data&gt;
       &lt;variable name=&quot;user&quot; type=&quot;com.example.User&quot;/&gt;
   &lt;/data&gt;
   &lt;LinearLayout
       android:orientation=&quot;vertical&quot;
       android:layout_width=&quot;match_parent&quot;
       android:layout_height=&quot;match_parent&quot;&gt;
       &lt;TextView android:layout_width=&quot;wrap_content&quot;
           android:layout_height=&quot;wrap_content&quot;
           android:text=&quot;@{user.firstName}&quot;/&gt;
       &lt;TextView android:layout_width=&quot;wrap_content&quot;
           android:layout_height=&quot;wrap_content&quot;
           android:text=&quot;@{user.lastName}&quot;/&gt;
   &lt;/LinearLayout&gt;
&lt;/layout&gt;
</pre>

### 데이터 바인딩하기
각각의 레이아웃 파일마다 바인딩 클래스가 만들어진다. 기본적으로 클래스의 이름은 레이아웃 파일의 이름에 따른다. 만약 activity_main.xml 이었다면 ActivityMainBinding 이라는 이름으로 생기게 된다.

<pre class="prettyprint">
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    val binding: ActivityMainBinding = DataBindingUtil.setContentView(
            this, R.layout.activity_main)

    binding.user = User("Test", "User")
}
</pre>

### Collection Data 바인딩하기
<pre class="prettyprint">
&lt;data&gt;
    &lt;import type=&quot;android.util.SparseArray&quot;/&gt;
    &lt;import type=&quot;java.util.Map&quot;/&gt;
    &lt;import type=&quot;java.util.List&quot;/&gt;
    &lt;variable name=&quot;list&quot; type=&quot;List&amp;lt;String&gt;&quot;/&gt;
    &lt;variable name=&quot;sparse&quot; type=&quot;SparseArray&amp;lt;String&gt;&quot;/&gt;
    &lt;variable name=&quot;map&quot; type=&quot;Map&amp;lt;String, String&gt;&quot;/&gt;
    &lt;variable name=&quot;index&quot; type=&quot;int&quot;/&gt;
    &lt;variable name=&quot;key&quot; type=&quot;String&quot;/&gt;
&lt;/data&gt;
&hellip;
android:text=&quot;@{list[index]}&quot;
&hellip;
android:text=&quot;@{sparse[index]}&quot;
&hellip;
android:text=&quot;@{map[key]}&quot;
</pre>

### 바인딩 어댑터
바인딩시 몇 속성들은 setter를 가지고 있지 않을 수 있다. 이런 상황에는 **BindingMethods** 어노테이션을 사용한다. 이 어노테이션은 클래스에 쓰인다.  
아래 예시에서는 android:tint 라는 attribute가 setImageTintList라는 메소드와 연관되어 있다. 대부분의 경우에는 안드로이드 프레임워크 클래스들의 세터를 다시 세팅할 필요는 없다. 
<pre class="prettyprint">
@BindingMethods(value = [
    BindingMethod(
        type = android.widget.ImageView::class,
        attribute = "android:tint",
        method = "setImageTintList")])
</pre>

### 양방향 데이터 바인딩
단방향 데이터 바인딩을 사용하면, 아래와 같이 변경에 반응하는 리스너와 값을 세팅할 수 있다.
<pre class="prettyprint">
&lt;CheckBox
    android:id=&quot;@+id/rememberMeCheckBox&quot;
    android:checked=&quot;@{viewmodel.rememberMe}&quot;
    android:onCheckedChanged=&quot;@{viewmodel.rememberMeChanged}&quot;
/&gt;
</pre>

양방향 바인딩은 위의 코드를 아래처럼 줄여 준다. @={} 을 사용하여 데이터 변경과 프로퍼티의 변경을 받고 유저의 업데이트를 동시에 받을 수 있다.
<pre class="prettyprint">
&lt;CheckBox
    android:id=&quot;@+id/rememberMeCheckBox&quot;
    android:checked=&quot;@={viewmodel.rememberMe}&quot;
/&gt;
</pre>

### Observable
데이터의 수정이 일어났을 때 그것을 감지할수 있게 하는 것이다.  
observable 클래스에는 세가지 타입이 있는데, object, fields, collectios이다.  
이중 하나의 observable 데이터에 변화가 생기면 UI가 자동으로 업데이트된다.

뒤에서 변하는 변화를 감지하기 위해서 레이아웃 변수에 **Observable** 을 구현하여 사용할 수 있다. 기본적으로 BaseObservable을 쓰고, @Bindable 어노테이션을 쓴다.

<pre class="prettyprint">
class LoginViewModel : BaseObservable {
    // val data = ...

    @Bindable
    fun getRememberMe(): Boolean {
        return data.rememberMe
    }

    fun setRememberMe(value: Boolean) {
        // Avoids infinite loops.
        if (data.rememberMe != value) {
            data.rememberMe = value

            // React to the change.
            saveData()

            // Notify observers of a new value.
            notifyPropertyChanged(BR.remember_me)
        }
    }
}
</pre>

