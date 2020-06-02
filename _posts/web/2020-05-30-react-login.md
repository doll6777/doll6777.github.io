---
layout: post
title:  React Google Login 구현하는 법
excerpt: "web"
tags: [programming, web]
permalink: /os/:year/:month/:day/:title/
category : Web
---

- https://console.developers.google.com/apis/ 구글 API 콘솔에서 ClientID를 발급받는다.

![google-console](/assets/web/2020-05-30-react-login/001.png)

- yarn add react-google-login

<pre class="prettyprint">
import React, { Component } from &#x27;react&#x27;
import { Button, Image } from &#x27;semantic-ui-react&#x27;
import { GoogleLogin } from &#x27;react-google-login&#x27;;

export default class MyPage extends Component {

    state = {
    }

    render() {
        return (&#x3C;div&#x3E;
              &#x3C;GoogleLogin
                clientId={myClientID}
                onSuccess={(res)=&#x3E;{
                    console.log(res);
                }}
                onFailure={(err)=&#x3E;{
                    console.log(err);
                }}
                &#x3E;
              &#x3C;/GoogleLogin&#x3E;
        &#x3C;/div&#x3E;);
    }
}
</pre>

> idpiframe_initialization_failed 에러 나는 경우

- localhost:3000 에서 google login 사용하니 에러가 발생했다. 이런 경우엔 google api console에서 허용할 url에 http://localhost:3000 을 추가해준다. 

- 그래도 해결되지 않는 경우엔 크롬의 경우 설정 페이지에서 쿠키 관련 히스토리를 모두 삭제시키면 된다.


