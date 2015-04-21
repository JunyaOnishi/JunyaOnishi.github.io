---
layout: post
title: "scoopなるパッケージ管理ソフトがあったので使ってみた。"
date: 2015-04-21 17:50:50
categories: package_manager
---
### はじめに
事はすごく簡単で、windowsで.gitignoreを作るのめんどくさいなと言うことからだった。
-[gibo](https://github.com/simonwhitaker/gibo)
が便利らしいということを聞いて、導入しようと思った時目に入ったのが

> scoop install gibo

でした、ん？なにこれ
何やらミニマムなChocolateyのようなものらしい
-[scoop](http://scoop.sh/)

### Chocolateyと違う点
- デフォルトインストール場所

{% highlight bash shell scripts %}
  ~/appdata/local/
{% endhighlight %}

- 管理者権限は必要ない
: UserのLocalにインストールするからって事ですね。

- パス汚染しない
: んんん？このへんは少し異議がある人も居る気がする。
: 変にユーザー領域にパス通ってるほうが困ったりするんですよ

- NuGetは使わない
: 依存解決するソフトではなく、単一のインストーラーってことですね。

- Git使ってる
: わりと簡単に自分のバケットを持つことができるので社内管理ソフトとして使う分には便利かもしれません。

- バージョン指定はできない
: forkしたらすぐ出来る様になる気がします

パッケージがあったらChocolateyで良いんじゃないかな……