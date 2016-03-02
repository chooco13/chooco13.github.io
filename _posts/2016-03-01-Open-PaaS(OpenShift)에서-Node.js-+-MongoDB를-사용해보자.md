---
layout: post
title:  Open PaaS(OpenShift)에서 Node.js + MongoDB를 사용해보자
date:   2016-03-01 23:28:09 +0900
categories: blog
description: "Open PaaS및 OpenShift에서 Node.js와 MongoDB를 사용하기 위한 설정법에 대해서 설명합니다."
---
### Open PaaS에 대하여

MEAN 스택 공부를 위해 무료 호스팅 이나 Paas 가 없을까 찾아보다가 알게되었습니다.

`Open PaaS` 는 클라우드 기반 SW개발환경을 무료로 지원해주는 사이트 입니다.  
2013년부터 서비스를 시작한 것 같은데 이제야 알게되어 사용해보려 합니다.  
[Open PaaS 클라우드 지원센터][Open-PaaS]  

국내에서 지원해 주는 서비스라 자료가 부족하지 않을까 걱정하였는데 Open PaaS 는 Red Hat 의 `OpenShift` 를 사용합니다.  
(그럼 무슨 차이가 있을까 해서 찾아봤는데 사실 저 같은 개인 개발자 용도로는 별다른 차이가 없는 것 같습니다. 나중에라도 큰 차이를 알게되면 추가하겠습니다.)  
[OPENSHIFT][Openshift]

## 환경설정

저는 우분투로 진행하여서 지원센터의 [리눅스 가이드][linux-guide] 를 따라 진행하였습니다.  
다른 운영체제로 하시는 분은 해당 운영체제 가이드를 참고하시면 될 것 같습니다.  
(참고로 웹에서 프로젝트를 생성하면 node.js를 선택할 수 없더군요.)

먼저 진행을 위해서는 루비를 설치해야 합니다.

{% highlight shell %}
sudo apt-get install ruby-full  
{% endhighlight %}

루비 설치가 완료되면 Client tool을 설치하고 업데이트 합니다  
(저는 업데이트 항목이 없었으나 일단 하라고 하니 하는게 좋을 것 같습니다.)

{% highlight shell %}
sudo gem install rhc  
gem update rhc  
{% endhighlight %}

다음은 공개키를 생성하고 설정해야 합니다.  
{% highlight shell %}
ssh-keygen -t rsa
{% endhighlight %}  
디렉토리를 기본경로로 사용하고싶으시면 엔터를 누르시고  
패스워드 자유롭게 입력하신 후 확인까지 완료해주시면 되겠습니다.

그 이후에는 공개키를 통한 연결 과정이 필요합니다.  
{% highlight shell %}
rhc setup --server broker.cloudsc.kr
{% endhighlight %}  
사이트 가이드를 따라가면 그대로 복붙하면 안되는 현상이 있는데 겠--가 -–로 적혀있어 그렇습니다.  
(집중해서 보지 않으면 차이가 느껴지지 않습니다.)  
계정 email와 비밀번호를 입력한 후 y 키를 계속 눌러주시면 완료됩니다.

사이트 가이드는 여기서 마무리 됩니다.

## Node.js + MongoDB 어플리케이션 시작하기

이제 node.js로 어플리케이션을 생성하도록 하겠습니다.
{% highlight shell %}
rhc app create '어플리케이션 이름' nodejs-0.10
{% endhighlight %}  

저는 myapp이라고 이름을 지었습니다. 그러면 아래와 같이 되겠죠  
{% highlight shell %}
rhc app create myapp nodejs-0.10
{% endhighlight %}  


MongoDB를 사용하기 위해 카트리지를 추가합니다.
{% highlight shell %}
rhc cartridge add mongodb-2.4 -a myapp
{% endhighlight %}  

모든 과정이 마무리 되었습니다.  

자신의 프로젝트 주소로 이동해보면 다음과 같이 나오게 됩니다.
(자신의 프로젝트 주소는 http://'어플리케이션 이름'-'계정도메인(가입 시 설정)'.cloudsc.kr/ 입니다.)
![result]({{ site.url }}/images/1/result.png)

수고하셨습니다.

[Open-PaaS]: http://openpaas.cloudsc.kr
[Openshift]: https://www.openshift.com/
[linux-guide]: http://openpaas.cloudsc.kr/start/commandLineLinux
