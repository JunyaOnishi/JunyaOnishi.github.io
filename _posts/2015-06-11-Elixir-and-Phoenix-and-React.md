---
layout: post
title: "Elixir開発環境の作成"
date: 2015-06-11 18:36:00
categories: Elixir Phoenixframework React
---
## はじめに
- [Elixir](http://elixir-lang.org/)
: Elixirは[ErlangVM(BEAM)](http://www.erlang.org/)上で動作する、Rubyライクな関数型言語です。
: Erlandは、[論理型言語](http://ja.wikipedia.org/wiki/%E8%AB%96%E7%90%86%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0)っぽく書かれており、クセが強く書きづらいものでした。
: しかし、大規模FPSゲームなどの舞台裏で使用されており、元々ゲーム畑出身である私もその存在は耳にしておりました。
: Rubyライクな文法で書ける、関数型言語というだけでキュンときて学習を決意。

- [Phoenix framework](http://www.phoenixframework.org/)
: Elixir上で動作するframeworkです。
: Railsの影響を強く受けているというか、Railsだコレって作りになっています。
: Railsは動かないので嫌いな私も、この子にはにっこり。

## 環境作成
今回はRDBを使用する関係上、Vagrantで環境を作っていきます。

#### Vagrantfileを作成

{% highlight ruby lineones %}
# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update = false
  config.vm.network "forwarded_port", guest: 4000, host: 7000
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  end
end
{% endhighlight %}

#### Vagrantを起動、ssh接続

{% highlight bash shell scripts %}
<C:/(Elixirプロジェクトフォルダ)>
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 4000 => 7000 (adapter 1)
    default: 22 => 2222 (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: Warning: Connection timeout. Retrying...
==> default: Machine booted and ready!
GuestAdditions 5.0.0 running --- OK.
==> default: Checking for guest additions in VM...
==> default: Checking for host entries
==> default: Configuring and enabling network interfaces...
==> default: Mounting shared folders...
    default: /vagrant => (共有フォルダ)
==> default: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> default: flag to force provisioning. Provisioners marked to run always will still run.
$ vagrant ssh
{% endhighlight %}

#### nodejsをインストール

{% highlight bash shell scripts %}
$ curl -sL https://deb.nodesource.com/setup_0.12 | sudo bash -
$ sudo apt-get install -y nodejs
$ node -v
v0.12.7
$ npm -v
2.13.3
{% endhighlight %}

#### elixirをインストール

{% highlight bash shell scripts %}
$ wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb && sudo dpkg -i erlang-solutions_1.0_all.deb
$ sudo apt-get update
$ sudo apt-get install -y elixir
{% endhighlight %}

#### phoenixframeworkをmixる

{% highlight bash shell scripts %}
$ mix local.hex
$ mix archive.install https://github.com/phoenixframework/phoenix/releases/download/v0.15.0/phoenix_new-0.15.0.ez
$ cd /usr/src
$ sudo mix phoenix.new hello_phoenix
We are all set! Run your Phoenix application:
    $ cd hello_phoenix
    $ mix ecto.create
    $ mix phoenix.server
You can also run your app inside IEx (Interactive Elixir) as:
    $ iex -S mix phoenix.server
{% endhighlight %}

おしまい！