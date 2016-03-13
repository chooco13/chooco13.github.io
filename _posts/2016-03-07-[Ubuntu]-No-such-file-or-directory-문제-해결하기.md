---
layout: post
title: "&#91;Ubuntu&#93; node&#58; No such file or directory 문제 해결하기"
date: 2016-03-07 16:00:00 +0900
categories: blog
description: Ubuntu(우분투)에서 Node 사용 시 발생하는 No such file or directory 문제의 원인과 해결하는 방법에 대해 알아보겠습니다.
---

### 원인

There is a naming conflict with the node package (Amateur Packet Radio Node Program), and the nodejs binary has been renamed from node to nodejs. You'll need to symlink /usr/bin/node to /usr/bin/nodejs or you could uninstall the Amateur Packet Radio Node Program to avoid that conflict.

출처 : [http://stackoverflow.com/a/18130296](http://stackoverflow.com/a/18130296)

매우 간단하게 요약하면 이미 `node` 라는 이름이 붇은 패키지 프로그램이 있어 충돌가능성이 있기 때문에
`nodejs` 를 사용한다는 것입니다.

그래서 저는  `nodejs`로 사용해 왔는데
오늘 공부하면서 나온 gulp 라는 툴은 node를 잡지 못하는 일이 있어 찾아보니 해당 위치에 node가 없어서 일어난 일이였습니다.

### 해결방법

터미널에서 다음 명령어를 입력해 주시면 됩니다.

{% highlight shell %}
sudo ln -s /usr/bin/nodejs /usr/bin/node
{% endhighlight %}

이제 정상적으로 동작할 것 입니다.
