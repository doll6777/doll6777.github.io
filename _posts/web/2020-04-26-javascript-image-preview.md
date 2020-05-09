---
layout: post
title:  React에서 Input의 Image Preview 구현하기
excerpt: "web"
tags: [programming, web]
permalink: /os/:year/:month/:day/:title/
category : Web
---

React에서 input 팝업에서 이미지를 가져온 후 미리보기 이미지를 구현하는 법은 다음과 같다.  
Image 컴포넌트는 semantic ui react의 것을 사용하였다.

<pre class="prettyprint">
    import { Image } from &#x27;semantic-ui-react&#x27;

    &#x3C;Image src={this.state.imageUrl} size=&#x27;small&#x27; /&#x3E;
    &#x3C;input 
        type=&#x27;file&#x27; label=&#x27;Upload&#x27; accept=&#x27;.png&#x27; 
            onChange={(event)=&#x3E;{ 
            this.setState({...this.state, 
            imageUrl: window.URL.createObjectURL(event.target.files[0]),
            });
            }}
        ref={(ref) =&#x3E; this.fileUpload = ref}
    /&#x3E;
</pre>