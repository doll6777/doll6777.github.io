---
layout: post
title: Jackson Library에서 다형성 지원하여 역직렬화 하기
excerpt: "Java"
tags: [programming, effective, java]
permalink: /android/:year/:month/:day/:title/
category : Java
---

Java에서 Json 형식을 역직렬화 하거나 직렬화 할 때 Jackson 라이브러리를 사용한다.  
이 때 고정된 스펙에 따라 Json을 역직렬화 하는것은 쉽다. 하지만 타입에 따라 부분이 달라지는 형식을 받는 방법은 무엇일까?  

예를 들어, 다음과 같이 agentValue 에 들어가야 할 타입이 바뀌는 것을 볼 수 있다. 이부분을 어떻게 받는쪽에서 쉽게 받을 수 있을까?  

내가 필요한 것은 여러가지 타입의 "CheckerDetectedValue" 를 type[BooleanValueAgentChecker, LongValueAgentChecker, LongValueAlarmChecker] 에 따라 다른 타입으로 받는것이다.  

### Json Example
(1)
```
"checkerDetectedValue": {
    "type": "BooleanValueAgentChecker",
    "value": [{
        "agentId": "agentIdtest",
        "agentValue": true}
    ]
},
```

(2)
```
"checkerDetectedValue": {
    "type": "LongValueAgentChecker",
    "value": [{
        "agentId": "agentIdtest",
        "agentValue": 1}
    ]
},
```

(3)
```
"checkerDetectedValue": {
    "type": "LongValueAlarmChecker",
    "value": 1
},
```

### How to deserialize?
상위 타입 클래스에 작업할 부분은 다음과 같다. jackson의 어노테이션을 사용해야 한다.  

### @JsonTypeInfo
어떤 프로퍼티를 보고 해당하는 타입으로 역직렬화 해야하는지를 설정해주는 부분이다. "type"이름으로 지정된 필드의 내용을 참조할 것이다.  
<pre class="prettyprint">
@JsonTypeInfo(
        use = JsonTypeInfo.Id.NAME,
        include = JsonTypeInfo.As.PROPERTY,
        property = "type")
</pre>

### @JsonSubTypes
type 프로퍼티에 적혀있는 이름과 매칭하는 클래스를 지정해 준다. "LongValueAgentChecker" 라는 이름으로 type이 지정되어 있다면, 그 클래스는 LongValueAgentCheckerDetectedValue로 지정하여 역직렬화 한다.  
<pre class="prettyprint">
@JsonSubTypes({
        @JsonSubTypes.Type(value = LongValueAgentCheckerDetectedValue.class, name = "LongValueAgentChecker"),
        @JsonSubTypes.Type(value = LongValueAlarmCheckerDetectedValue.class, name = "LongValueAlarmChecker"),
        @JsonSubTypes.Type(value = BooleanValueAgentCheckerDetectedValue.class, name = "BooleanValueAgentChecker"),
        @JsonSubTypes.Type(value = DataSourceAlarmListValueAgentCheckerDetectedValue.class, name = "DataSourceAlarmListValueAgentChecker"),
})
</pre>

[전체 코드] 하위 타입을 받을 상위 타입 클래스 위에 어노테이션 정보를 지정해준다.  
<pre class="prettyprint">
@JsonTypeInfo(
        use = JsonTypeInfo.Id.NAME,
        include = JsonTypeInfo.As.PROPERTY,
        property = "type")
@JsonSubTypes({
        @JsonSubTypes.Type(value = LongValueAgentCheckerDetectedValue.class, name = "LongValueAgentChecker"),
        @JsonSubTypes.Type(value = LongValueAlarmCheckerDetectedValue.class, name = "LongValueAlarmChecker"),
        @JsonSubTypes.Type(value = BooleanValueAgentCheckerDetectedValue.class, name = "BooleanValueAgentChecker"),
        @JsonSubTypes.Type(value = DataSourceAlarmListValueAgentCheckerDetectedValue.class, name = "DataSourceAlarmListValueAgentChecker"),
})
public interface CheckerDetectedValue {
}
</pre>

이제 하위 타입 클래스에는 다음과 같이 작업한다. 

### @JsonTypeName
LongValueAgentCheckerDetectedValue로 파싱해야 할 클래스는 
@JsonTypeName("LongValueAgentChecker") 라는 이름을 가져야 하므로
클래스 위에 다음과 같이 적어준다. 다른 CheckerValue들도 이와같이 적어주면 된다.  

### @JsonCreator
만약 하위 타입에 받아야 할 정보들이 있다면 기본 클래스 생성자가 디폴트 생성자로 지정되기 때문에 제대로 받을 수 없다. 따라서 JsonCreator 어노테이션을 설정해 주어야 한다. 

<pre class="prettyprint">
@JsonTypeName("LongValueAgentChecker")
public class LongValueAgentCheckerDetectedValue extends AgentCheckerDetectedValue<Long> {
    
    @JsonCreator
    public LongValueAgentCheckerDetectedValue(List<DetectedAgent<Long>> value) {
        super(value);
    }
}
</pre>

Reference: https://gist.github.com/christophercurrie/8939489