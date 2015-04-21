---
layout: post
title: "Chefをぶん投げてitamaeに乗り換えたお話"
date: 2015-04-07 9:50:50
categories: itamae
---
### はじめに
ところでみなさん、[Chef](https://www.chef.io/)は好きですか？
C言語コンパイルエラーで消耗してますか？
特に意味なく消されるWin用コードとStable(動かない)に消耗してませんか？

#### もう我慢の限界だね！俺はプロビジョニングを楽にしたいだけなんだ！

### [itamae](https://github.com/itamae-kitchen/itamae)を使おう
itamaeはクックパッドの社内ツールだったものがOSS化したものです。
凄い簡単にしたChef見たいな、学習コストも低く、導入障害点も少ない優れたツールです。

> [Ansible](http://www.ansible.com/)? ドザーに取っては知らない子ですな。

### チートシート
- directory
: 指定ディレクトリを作成する
{% highlight ruby linenos %}
directory "作成場所/ディレクトリ名" do
  mode "パーミッション"
  owner "作成ユーザー"
  group "所属グループ"
end
{% endhighlight %}

- execute
: シェルコマンドを実行する。
{% highlight ruby linenos %}
execute "update" do
  user "root"
  command "yum -y update"
end
{% endhighlight %}

- file
: 指定ファイルを作成する
{% highlight ruby linenos %}
file "ファイルパス" do
  content "ファイルの内容"
end
{% endhighlight %}

- git
: git cloneを実行する
{% highlight ruby linenos %}
git '対象ディレクトリパス' do
  repository "対象のgitリポジトリ"
end
{% endhighlight %}

- link
: シンボリックリンクを作成する
{% highlight ruby linenos %}
link "リンク先パス" do
  to "リンク元パス"
end
{% endhighlight %}

- package
: 指定パッケージをインストールする
{% highlight ruby lineons %}
package "パッケージネーム" do
  version "指定バージョン"
  options "オプション"
end
{% endhighlight %}

- remote
: ファイルを転送する
{% highlight ruby linenos %}
remote_file "対象ディレクトリパス(転送先)" do
  owner "root"
  group "root"
  source "転送元ソース"
end
{% endhighlight %}

- template
: erb(テンプレートを転送する)
{% highlight ruby linenos %}
template "/tmp/template" do
  source "テンプレートソース.erb"
  variables("埋め込む値")
end
{% endhighlight %}
