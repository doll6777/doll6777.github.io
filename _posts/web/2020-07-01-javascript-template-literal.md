---
layout: post
title: Javascript의 Tagged Template Literal
excerpt: "web"
tags: [programming, web]
permalink: /web/:year/:month/:day/:title/
category : web
---

### Template Literal이란?
> https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals

백틱 (` `)을 이용하여 표현식을 넣을 수 있다. 템플릿 리터럴 앞에 어떠한 표현식(태그) 가 있다면, 템플릿 리터럴은
태그가 지정된 템플릿으로 불리게 된다.  

### Tagged Templates
태그를 사용하면 템플릿 리터럴을 함수로 파싱 할 수 있다. 태그 함수의 첫 번째 인수는 문자열 값의 배열을 포함한다.  
나머지 인수는 표현식과 관련된다. 결국 함수는 조작 된 문자열을 반환 할 수 있다.  

- What's the output?
<pre class="prettyprint">
function getPersonInfo(one, two, three) {
  console.log(one);
  console.log(two);
  console.log(three);
}

const person = 'Lydia';
const age = 21;

getPersonInfo`${person} is ${age} years old`;
</pre>

A: "Lydia" 21 ["", " is ", " years old"]  
*B: ["", " is ", " years old"] "Lydia" 21*  
C: "Lydia" ["", " is ", " years old"] 21  