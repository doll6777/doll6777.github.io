---
layout: post
title: Firebase collection add시 permission error 나는 경우
excerpt: "Android"
tags: [android, programming, thread, handler]
permalink: /android/:year/:month/:day/:title/
category : 안드로이드
---

Firebase 연동 중 collection에 새로운 아이템을 insert 하려고 하는데  
PERMISSION_DENIED 라는 에러 발생. 

https://stackoverflow.com/questions/48169705/getting-firestore-permission-denied-while-integrating-firestore-in-react-nativ

구글링하여 찾아보니 firebase 콘솔에서 Database -> 규칙에 들어가

<pre class="prettyprint">
service cloud.firestore {
  match /databases/{database}/documents {
    match /todos/{document=**} {
      allow read, write: if true;
    }
  }
}
</pre>

이렇게 설정하니 해결되었다.

