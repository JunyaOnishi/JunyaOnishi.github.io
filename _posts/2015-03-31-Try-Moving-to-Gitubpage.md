---
layout: post
title: "GithubPageに引っ越ししてみた"
date: 2015-03-31 14:17:50
categories: jekyll
---
### ことのあらまし
以前はtumblr.に[ブログ](http://gruntjo.tumblr.com/)を作っていたのですが、なんか技術屋っぽくなくね！？と言うことでGithubに移行して、テンプレートエンジンとしてjekyllを使用してみることにしました。

### なにがちがうの？
今度はsyntax highlighterも搭載し、Python, Ruby, md記法ととてもえんじにあっぽい構成になっています！

#### phpでシングルトンパターン置いておきますね

{% highlight php5 linenos %}
<?php
// Singleton.php
trait Singleton
{
    private static $instance = [];

    private function __construct()
    {
    }

    public static function getInstance()
    {
        $class = get_called_class();
        if (!isset(self::$instance[$class])) {
            self::$instance[$class] = new $class;
        }
        return self::$instance[$class];
    }

    public final function __clone()
    {
        throw new \Exception('Clone is not allowed against' . get_class($this));
    }
}
{% endhighlight %}

{% highlight php5 linenos %}
<?php
// call.php
require_once('Singleton.php');

class CallSingleton
{
  use Singleton;
}

$test = CallSIngleton::getInstance();
var_dump($test);
{% endhighlight %}
