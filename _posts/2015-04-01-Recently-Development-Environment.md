---
layout: post
title: "最近の開発環境"
date: 2015-04-01 10:16:50
categories: DevelopTools
---
### 使っているもの
- [ConEmu](https://github.com/Maximus5/ConEmu)(コンソール)
: タブ付きのコンソール、Ansi Color対応で非常に使いやすい
: 今のところはこのコンソールから変更するつもりがない

- [Nyagos](https://github.com/zetamatta/nyagos)(シェル)
: 日本語シェルとしては貴重なのとlua拡張が使えるのでガシガシカスタマイズできるので気に入っている
: .nyagosを育てるのが楽しい
{% highlight lua linenos %}
-- .nyagos
-- This is a sample .nyagos written with Lua.
-- Edit and put it on %USERPROFILE% or %HOME%

-- Simple Prompt for CMD.EXE
set{
    PROMPT='$L$P$G$_$$$s'
}

-- Coloring Prompt for NYAGOS.exe
local prompter=nyagos.prompt
nyagos.prompt = function(this)
    return prompter('$e[31;40;1m'..this..'$e[37;1m')
end

-- Use $PATH and ${PATH} Thanks nocd5!
-- http://qiita.com/nocd5/items/aa155e91a6eef58b3940
local _filter = nyagos.filter
nyagos.filter = function(cmdline)
  local.post = cmdline:gsub('${([%w_()]+)}', '%%1%%')
  post = post:gsub('$([%w_()])', '%%1%%')
  return _filter(post)
end
-- vim:set ft=lua: --
{% endhighlight %}
- [Chocolatey](https://chocolatey.org/)(パッケージ管理ソフト)
: 便利だが、ゴタ付いている界隈で使用すると血を見ることになる
: Ruby、VirtualBox辺りは特に警戒
- [Sublime Text3](http://www.sublimetext.com/3) (メインエディタ)
: 殆どIDE化している。
: 自分の好きな見た目で作業ができるというのは非常に幸福なことなのです。
- [Atom](https://atom.io/)(サブエディタ)
: jekyllを使う上で相性が良かったのとSublime text3 build 3080事件があったので作業ができるようにサブエディタとして使っている
- [Vim](http://cream.sourceforge.net/)(コンソールでのエディタ)
: 本当にちょっとした変更や大量のテキストを修正したいときはConsoleからVimを起動している。
: [Kaoriya](http://www.kaoriya.net/software/vim/) 版は微妙に使いにくいのでCream for Vimを使用してます。
: 下記コマンドで入るものと一緒です。
{% highlight Bash shell scripts linenos %}
$ cinst -y vim
{% endhighlight %}
> 追記:Cream VimがUpdateでウンコ化したので、いまはKaoriya版をつかってます。
- [VirtualBox](https://www.virtualbox.org/)(仮想環境作成)
: 問題児、Stable(動かない)をよくやる。
: 現状の選択肢がこれしか無いので、仕方なく使っている。
: > どこがStableだ言ってみろォ！
- [Vagrant](https://www.virtualbox.org/)(仮想環境作成)
: 問題児のお守りをさせている、しょっちゅう仮想BOXを壊されているかわいそうな子
- [Docker](https://www.docker.com/)(仮想環境作成)
: ちゃっちゃとアプリケーション環境が作れるので、急いで挙動確認する時に使っている
- [gof](https://github.com/mattn/gof)(Fuzzy Finder)
: Fuzzy Finderが欲しかったところで見つけた、非常に便利に使わせて頂いております
- [Grunt](http://gruntjs.com/)(タスクランナー)
: 最近はConEmuで背後で動かしながら作業を行っている。
: ライブリロード、コンパイル、アップロード、サイトマップの作成全部これで行っている
- [jade](http://jade-lang.com/)
: 使いやすい、似たようなページを大量に作るときの相棒
- [Coffeescript](http://coffeescript.org/)
: 究極のゆるふわ記法
: 彼女にしたい言語No1

### 使わなくなったもの
- [ckw mod](http://ckw-mod.github.io/)
: 非常に使いやすくて気に入っていたのだが、
: vim弄ってると盛大に画面が崩れるのでサヨウナラの運びとなった

- [Node.js](https://nodejs.org/)
: [お家騒動](http://yosuke-furukawa.hatenablog.com/entry/2014/12/25/104300)でゴタゴタしてて、本家の発展性が無いので、ローカルにしか使ってないし軽いので[io.js](https://iojs.org/ja/)に乗り換えた

- [jenkins](https://jenkins-ci.org/)
: テストファーストがまだまだ徹底できないので事後にテストを書くことが多かった
: 結果的にデプロイにしか使っていなく、余りローカルに環境を用意しておく意味が無いので
: [werker](http://wercker.com/)に乗り換えた、事後でテストも書けるし取り敢えずデプロイするのにも使える
- [Chef](https://www.chef.io/)&[Berkshelf](http://berkshelf.com/)
: まともに動いた試しがない。
: ChefDKすら安心感がない、そもそもBerkshelf動かない

### 新しく使うようになったもの
- [Bower](http://bower.io/)
: 最近は「オリジナルさ」よりも速度重視で「車輪の再発明」はしないをコンセプトに組み立てている。
: これ以上無いぐらい強力な相棒となっている
- [Browserify](http://browserify.org/)
: commonJSスタイルを徹底するようになったので使うようになった魔法の杖
: 非常に使いやすいので楽しんで書いている
- [Slack](https://slack.com/)
: とにかく連携機能が強力なので、使っている基本的に開発仲間との連絡とか、自分用にビルドが通ったとか、エアコンをオンオフしたりとか、そういう報告を受けるのに使っている。
- [itamae](https://github.com/itamae-kitchen/itamae)&[vagrant-itamae](https://github.com/chiastolite/vagrant-itamae)
: きちんと動く喜び。これに尽きる。(gemのAnsiが文句言ってくるけど、それはAnsiのせいだし)
: こんな感じでかなり緩く書ける
{% highlight ruby linenos %}
# Vagrantfile
# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure(2) do |config|
  config.vm.box = "chef/centos-7.0"
  config.vm.box_check_update = false
  config.vm.network "forwarded_port", guest: 80, host: 8083
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    # 通信が異常に重くなるバグの回避
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  end
  config.vm.provision :itamae do |config|
    config.sudo = true
    config.recipes = ['cookbook/recipe.rb']
  end
end
{% endhighlight %}

{% highlight ruby linenos %}
# cookbook/recipe.rb
execute 'yum-update' do
  command 'yum update -y'
end

package "httpd" do
  action :install
end

package "mariadb" do
  action :install
end

package "mariadb-server" do
  action :install
end

package "php" do
  action :install
end

package "php-devel" do
  action :install
end

package "php-pdo" do
  action :install
end

package "php-mbstring" do
  action :install
end

package "php-xml" do
  action :install
end

package "php-xmlrpc" do
  action :install
end

package "php-mysql" do
  action :install
end

execute 'remove-var-www-html' do
  command 'rm -drf /var/www/html'
end

link "/var/www/html" do
  to "/vagrant/"
end
{% endhighlight %}
