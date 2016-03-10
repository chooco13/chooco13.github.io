---
layout: post
title: "&#91;OpenShift&#93; Node.js에서 bcrypt 사용시 죽는 문제 해결하기"
date: 2016-03-10 18:00:00 +0900
categories: blog
description: "OpenShift를 이용하여 서비스한 Node.js에서 bcrypt를 사용할 경우 503 에러로 죽는 문제를 해결하는 방법에 대해 알아보도록 하겠습니다"
---

## 증상

저는 [MEAN 스택을 사용한 모던 웹 개발 입문][book] 이란 책을 통해 공부하고 있습니다.  

오늘은 bcrypt 사용법에 대한 내용을 배웠는데
로컬에서는 그렇게 잘 되던 프로젝트를 OpenShift에 올리니 사이트가 먹통이 되었습니다.
`503 service temporarily unavailable` 만 계속 뿜어댔습니다.  

OpenShift 가이드에서도 딱히 다뤄줄 내용이 없는지 503 에러가 발생할 경우 서비스를 재시작 해보라고만 언급되어있었습니다.
(아무래도 503 에러가 나온것은 사용자 문제라서 그런것 같습니다.)

그래서 어디에서 에러가 나는지라도 알아보자 싶어서 `rhc tail` 명령어를 찾게 되었습니다.  
(tail 뒤에 어플리케이션이름을 붙어 주시면 됩니다. 예 : rhc tail myapp)

명령어를 실행해보니 아래의 에러문이 반복해서 나오고 있었습니다.  
{% highlight shell %}
DEBUG: Starting child process with 'node server.js'
/var/lib/openshift/...secret.../app-root/runtime/repo/node_modules/bcrypt/node_modules/bindings/bindings.js:83
throw e
^
Error: /lib64/libc.so.6: version `GLIBC_2.14' not found (required by /var/lib/openshift/app-id/app-root/runtime/repo/node_modules/bcrypt/build/Release/bcrypt_lib.node)
//... 이하 생략
{% endhighlight %}  

## 해결방법

사실 특별한 해결방법이 없습니다. 오늘 하루종일 찾아봤는데 없었습니다.  

그나마 제가 찾은 한가지 방법은 위의 문제를 해결하는 것이 아니라, 포기하고 [bcrypt][bcrypt] 가 아니라 [bcrypt-nodejs][bcrypt-nodejs] 를 사용하는 것이였습니다.

bcrypt-nodejs는 node.js를 위해 순수 javascript로 구현되어 다른 요소에 종속 할 필요 없이 사용 할 수 있습니다.

설치하는 방법은 다른 nodejs 패키지와 마찬가지로 npm을 통해 가능합니다.  
{% highlight shell %}
sudo npm install --save bcrypt-nodejs
{% endhighlight %}

또한 문법 차이도 거의 없기 때문에 수정해 줄 부분은 require 뿐입니다.
{% highlight JavaScript %}
// var bcrypt = require('bcrypt')
var bcrypt = require('bcrypt-nodejs')
{% endhighlight %}

이제 git을 통해 PaaS 서버에 올리면 정상적으로 동작할 것입니다.

### 추가 내용

좀 더 공부하다 보니 hash 함수에서 차이를 발견했습니다.  

{% highlight JavaScript %}
hash(data, salt, cb) // bcrypt
hash(data, salt, progress, cb) // bcrypt-nodejs
{% endhighlight %}

pregress 부분을 null로 넣어주시면 같게 동작합니다.  
자세히 보고 싶으신분들은 위의 링크를 통해 들어가시면 되겠습니다.


[bcrypt]: https://www.npmjs.com/package/bcrypt
[bcrypt-nodejs]: https://www.npmjs.com/package/bcrypt-nodejs
[book]: http://book.naver.com/bookdb/book_detail.nhn?bid=8779083
