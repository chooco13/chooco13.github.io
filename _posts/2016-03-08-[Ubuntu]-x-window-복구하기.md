---
layout: post
title: "&#91;Ubuntu&#93; x window 복구하기"
date: 2016-03-08 16:00:00 +0900
categories: blog
description: ubuntu 사용 중 x window 가 깨졌을 때 어떻게 복구하는지에 대해 알아보겠습니다.
---

## 증상

- Ubuntu로 부팅을 했을 때 gui인 x window가 아닌 tty1 으로 자동 이동됨
- x window 가 깨져있어 Ctrl + Alt + F7 을 눌러도 _ 만 깜박이고 반응이 없음

## 해결방법

아무 tty 터미널에서 다음 명령어를 입력해 주시면 됩니다.

먼저 x window 를 실행시켜보라는 글이 있어서 `sudo startx` 명령어를 실행해보았지만 부분적으로 gui가 살아나긴 했으나 바탕화면외에는 아무것도 할 수 없었습니다.

그래서 조금 더 찾아보니 아래의 명령어를 찾게 되었습니다.  
{% highlight shell %}
sudo dpkg-reconfigure ubuntu-desktop
{% endhighlight %}  
이후에 재부팅 해주시면 이제 정상적으로 동작할 것 입니다.

출처 : [http://ubuntuforums.org/showthread.php?t=863429](http://ubuntuforums.org/showthread.php?t=863429)
