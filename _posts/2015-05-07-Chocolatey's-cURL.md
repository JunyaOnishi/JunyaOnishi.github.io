---
layout: post
title: "Chocolateyのcurlに泣かされた話"
date: 2015-05-07 10:25:50
categories: Chocolatey
---
### 問題点
連休明けなのでcup -y allをしたら、色々コケたので原因を調べるとcurlでコケているらしい事がわかった。

{% highlight bash shell scripts %}
<C:/>
$ where curl
C:\ProgramData\chocolatey\bin\curl.exe
{% endhighlight %}

マジでッ！？  
Chocolatey同梱のパッケージが優先されて使われていた。

### 解決策

- curlのある場所にcurl-ca-bundle.crtを加えてやれば良い

{% highlight bash shell scripts %}
<C:/>
$ cd C:\ProgramData\chocolatey\bin\
<C:/ProgramData/chocolatey/bin/>
$ wget http://curl.haxx.se/ca/cacert.pem
$ rename cacert.pem curl-ca-bundle.crt
{% endhighlight %}

リンク切れてるかも知れないので、コピペはせず

- [http://curl.haxx.se/docs/caextract.html](http://curl.haxx.se/docs/caextract.html) から持ってきたほうが安全
