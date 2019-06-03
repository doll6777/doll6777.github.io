---
layout: post
title: 안드로이드 Realm DB 사용하기
excerpt: "Android"
tags: [android, programming, thread, handler]
permalink: /android/:year/:month/:day/:title/
category : 안드로이드
---

# Android Realem 사용하기

### Realm 데이터베이스란? 
Relam사에서 만든 Mobile Databas이다. SQLite 다음으로 많이 사용된다.  
Realm은 속도가 빠르고 사용성이 편하다고 주장한다. 

### Realm의 단점?
다중 쓰레드에서 Realm의 객체를 관리하기 힘들다. Realm 객체는 쓰레드 간 직접 전달이 불가능하다. 

# Realm 사용 예제
*각 예제는 코틀린언어로 작성되어 있습니다.*

### 1. 라이브러리 설정
안드로이드 프로젝트는 두개의 Project의 build.gradle과 App레벨의 build.gradle을 가지고 있다.
이 두가지 파일에 다음 설정을 해주어야 한다.

top-level build.gradle
<pre class="prettyprint">
buildscript {
    ext.kotlin_version = '1.3.31'
    repositories {
        google()
        jcenter()
        
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.4.0'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        classpath 'io.realm:realm-gradle-plugin:5.2.0'
    }
}
</pre>

app-level build.gradle
<pre class="prettyprint">
apply plugin: 'realm-android'
</pre>

### 2. 모델 생성
RealmObject 객체를 상속받는다.

<pre class="prettyprint">
class BirdInfo : RealmObject() {
    var name: String = ""
    var level: Int = 0
    var hp = 100
    var happy = 100
    var angry = 100
}
</pre>

### 3. Realm 초기화 및 객체생성

<pre class="prettyprint">
    Realm.init(this)
    realm = Realm.getDefaultInstance()
</pre>

### 4. Realm 객체 이용해 트랜잭션 하고 Query 하기

beginTransaction을 시작하고 commitTransaction으로 끝내며, 사이에서 데이터와 관련된 작업을 실행한다.

<pre class="prettyprint">
class BirdInfoRepository {

    fun getBirdInfo(context: Context): BirdInfo? {
        val ranContext = context.applicationContext as RanApplication
        val realm = ranContext.realm

        realm.beginTransaction()
        val info = realm.where(BirdInfo::class.java).findFirst()

        realm.commitTransaction()
        return info
    }

    fun createBirdInfo(context: Context) : BirdInfo?{
        val ranContext = context.applicationContext as RanApplication
        val realm = ranContext.realm

        realm.beginTransaction()
        val birdInfo = realm.createObject(BirdInfo::class.java)
        realm.commitTransaction()

        return birdInfo
    }

    fun updateBirdInfo(context: Context, birdInfo: BirdInfo) {
        val ranContext = context.applicationContext as RanApplication
        val realm = ranContext.realm

        realm.beginTransaction()

        val info = realm.where(BirdInfo::class.java).findFirst()
        info!!.name = birdInfo.name
        info.angry = birdInfo.angry
        info.happy = birdInfo.happy
        info.hp = birdInfo.hp
        info.level = birdInfo.level

        realm.commitTransaction()
    }
}
</pre>