---
layout: post
title: Broadcast Receiver
excerpt: "Android"
tags: [android, programming, thread, handler]
permalink: /android/:year/:month/:day/:title/
category : 안드로이드
---

## Broadcast Receiver
방송 수신자이고 이벤트 발생시 방송을 해준다. 이렇게 방송된 이벤트는 각 앱에서 필요한 방송 이벤트를 받아들일 수 있고, 이벤트에 대한 처리를 리시버를 통해서 한다.


### 방송 보내기
<pre class="prettyprint">
    Intent intent = new Intent("com.ran.myapplication.SEND_BROAD_CAST");
    intent.putExtra("boolValue", true);
    intent.putExtra("intValue", 123);
    intent.putExtra("stringlValue", "Intent String");
    sendBroadcast(intent);
</pre>

### 방송 받기
리시버에는 정적리시버와 동적리시버가 있다. 정적 리시버는 한번 등록하면 해제할 수 없다.  
정적 리시버는 AndroidManifest.xml에 등록해야 한다.

<pre class="prettyprint">
<receiver android:name=".TestReceiver"> 
<intent-filter> 
<action android:name="com.ran.myapplication.SEND_BROAD_CAST"/> 
</intent-filter> 
</receiver>
</pre>

리시버 클래스는 BroadcastReceiver를 상속받고 onReceive에서 Intent를 받아서 처리한다.

<pre class="prettyprint">

public void onReceive(Context context, Intent intent) {

String name = intent.getAction();
if(name.equals("com.ran.myapplication.SEND_BROAD_CAST")) {
    String value = intent.getStringExtra("stringValue"));
}
}
</pre>

동적 리시버는 등록와 해제가 자유롭다. AndroidManifest.xml에 추가로 입력 할 사항이 없다.


<pre class="prettyprint">
IntentFilter intentfilter = new IntentFilter();
intentfilter.addAction("com.ran.myapplication.SEND_BROAD_CAST");

BroadcastReceiver receiver = new BroadcastReceiver() {
    @Override
    public void onReceive(Context context, Intent intent) {
        String string = intent.getStringExtra("stringValue");
        Log.d(TAG, string);
    }
}
registerReceiver(receiver, intentFilter);

unregisterReceiver(receiver); // 해제해야 할 때 반드시 호출
</pre>