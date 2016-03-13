---
layout: post
title:  "&#91;OpenShift&#93; Node.js + MongoDB 어플리케이션 올리기"
date:   2016-03-04 18:00:00 +0900
categories: blog
description: OpenShift에 Node.js 와 MongoDB 를 사용하는어플리케이션 올리는 방법에 대해 알아보겠습니다.
---

이 글은 OpenShift Documentation와 구글링과 삽질을 통해 작성되었습니다.

생성된 어플리케이션이 없으시다면 이 글을 보시기 전에  
먼저 [Open PaaS(OpenShift)에서 Node.js + MongoDB를 사용해보자][post1] 를 보고  
진행해 주셔야 합니다.

저는 [MEAN 스택을 사용한 모던 웹 개발 입문][book] 이란 책을 통해 공부하고 있습니다.

이 책으로 진행해 가던 중 p.83 에서 CORS 문제에 막혀서 차라리 아예 PaaS를 통해 진행해보자 라는 마음을 가지고 local(localhost)에서 공부를 하다 PaaS로 옮겼습니다.  
이 과정 중에 어떠한 작업이 필요한지 적어보려 합니다.

책 예제를 보고 싶으신 분은 [https://github.com/dickeyxxx/mean-sample][book-sample] 을 참고해 주시면 되겠습니다. (ch.5 입니다.)


### Node.js 설정하기
먼저 `rhc app create` 명령어를 통해 자동생성된 파일들을 정리해 봅시다.  
openshift .git 폴더와 .openshift 폴더를 제외하고는 다 삭제해 주세요.  
그 후에 원래 local에서 진행하던 프로젝트를 옮겨 주시면 됩니다.  
혹시 모르니깐 백업해 두셔도 좋을 것 같습니다.

PaaS이 같은 API를 통해 직접 제공하게 될 경우 AngularJS에서는 `http://localhost:3000/api/posts` 이런 형식이 아닌 `/api/posts` 와 같은 형식으로 코드를 고쳐 주셔야 합니다.  
예를 들면 `$http.get('http://localhost:3000/api/posts')`가 `$http.get('/api/posts')`로 고쳐져야 합니다.

다음은 `package.json`에서 수정할 부분에 대해 설명해 드리겠습니다.
name과 dependencies에서는 수정할 부분이 없고 그 아래 부분만 참고하시면 되겠습니다.  
저희는 아직 publish(배포) 하지 않은 상태이므로 그걸 방지하기 위해 `"private": true` 를 입력해야만 동작합니다.

{% highlight Json %}
{
  "name": "socialapp",
  "dependencies": {
    "body-parser": "^1.15.0",
    "express": "^4.13.4",
    "mongoose": "^4.4.6"
  },

  "scripts": {
    "start": "supervisor server.js"
  },

  "private": true,
  "main": "server.js"
}
{% endhighlight %}

다음으로  `server.js` 에서는 두 가지 작업이 필요합니다.
먼저는 server.js로 통해 접속했을때 html 화면이 출력되도록 하는 부분이 필요합니다.
뒤에 옵션부분은 `path must be absolute or specify root` 라는 조건을 충족하기 위해서 필요합니다.

{% highlight JavaScript %}
app.get('/', function(req, res) {
  res.sendFile('layouts/posts.html', { root: __dirname })
})
{% endhighlight %}

그 다음에는 OpenShift에서 제공하는 환경변수를 이용하여 서버 포트와 IP를 설정해 주어야 합니다.  
|| 를 통하여서 local에서도 구동 가능하도록 구성되어 있습니다.

{% highlight JavaScript %}
var server_port = process.env.OPENSHIFT_NODEJS_PORT || 8080
var server_ip_address = process.env.OPENSHIFT_NODEJS_IP || '127.0.0.1'

app.listen(server_port, server_ip_address, function() {
  console.log("Listening on " + server_ip_address + ", server_port " + server_port)
});
{% endhighlight %}

### MongoDB 설정하기

`db.js` 에서(혹은 db관련 소스가 들어있는 부분에서) 다음 부분을 추가해 주시면 되겠습니다.
server.js 에서의 IP설정과 마찬가지로 local에서도 동작하도록 구성되어 있습니다.

{% highlight JavaScript %}
// 'localhost' 에서 동작할 때
var connection_string = '127.0.0.1:27017/social';

// OPENSHIFT 에서 동작하여 환경변수가 제공될 경우
if(process.env.OPENSHIFT_MONGODB_DB_PASSWORD){
  connection_string = process.env.OPENSHIFT_MONGODB_DB_USERNAME + ":" +
  process.env.OPENSHIFT_MONGODB_DB_PASSWORD + "@" +
  process.env.OPENSHIFT_MONGODB_DB_HOST + ':' +
  process.env.OPENSHIFT_MONGODB_DB_PORT + '/' +
  process.env.OPENSHIFT_APP_NAME;
}

mongoose.connect('mongodb://'+ connection_string, function(){ ... })//이하 생략
{% endhighlight %}

수고하셨습니다.

결과 소스코드는 [https://github.com/chooco13/openshift-node.js-example][result]에 올려 두었습니다.

[post1]: http://chooco13.github.io/blog/2016/03/01/Open-PaaS(OpenShift)%EC%97%90%EC%84%9C-Node.js-+-MongoDB%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%B4%EB%B3%B4%EC%9E%90.html
[book]: http://book.naver.com/bookdb/book_detail.nhn?bid=8779083
[book-sample]: https://github.com/dickeyxxx/mean-sample
[result]: https://github.com/chooco13/openshift-node.js-example
