---
layout: post
title:  "&#91;Ubuntu&#93; NVIDIA 그래픽카드 사용시 밝기 조절 안되는 문제 해결방법"
date:   2016-03-02 22:00:00 +0900
categories: blog
description: "Ubuntu(우분투)에서 NVIDIA 그래픽카드 사용시 밝기값은 변경되나 실절적인 밝기값이 조절되지 않는 문제를 해결한다."
---
얼마 전에 네이버에서 강연을 듣게 되었는데 강연하신 분께서 가능하면 Terminal로 작업할 수 있는 환경을 갖춰보라고 하셔서 ~~맥북이 쓰고싶지만 돈이 없으므로~~ Ubuntu를 설치하였습니다.  
이것 저것 공부를 하다보니 점점 Bash(터미널이라 하나요?) 기반으로 되있는게 편해질 때가 생기더군요

그런데 우분투에서 NVIDIA 그래픽카드 사용시 밝기값은 변경되나 실절적인 밝기값이 조절되지 않는 문제가 있어 한참 검색하다가 해결책을 찾았습니다.

그래픽카드 드라이버를 설치하지 않으셨다면 먼저 그래픽카드 드라이버를 설치해주세요.  
{% highlight shell %}
sudo apt-get install nvidia-current
{% endhighlight %}  

/usr/share/X11/xorg.conf.d 경로에 `20nvidia.conf` 파일을 생성해 주어야 합니다.

{% highlight shell %}
sudo nano /usr/share/X11/x852org.conf.d/20nvidia.conf
{% endhighlight %}  

nano 에디터에서 아래의 내용을 복사 해주시면 됩니다.  
BoardName은 알맞게 수정해 주시면 됩니다.

{% highlight shell %}
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "GeForce 410M"
    Option         "RegistryDwords" "EnableBrightnessControl=1"
EndSection
{% endhighlight %}  

저장하신 후 재부팅 혹은 로그아웃 후 로그인 해주시면 완료됩니다.

참고 링크 : [http://askubuntu.com/questions/472850/brightness-control-in-ubuntu-14-04-on-sony-vaio](http://askubuntu.com/questions/472850/brightness-control-in-ubuntu-14-04-on-sony-vaio)  
