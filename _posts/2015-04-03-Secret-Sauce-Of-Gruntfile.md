---
layout: post
title: "S3で静的サイトを作る際のGruntfile"
date: 2015-04-03 8:56:50
categories: Gruntfile
---
### はじめに

- HTMLは書きたくない
: [Jade](http://jade-lang.com/)を使いましょう
- CSSは書きたくない
: [Stylus](http://learnboost.github.io/stylus/)+[nib](http://tj.github.io/nib/)で書いていきます。
: 正直[Sass](http://sass-lang.com/)+[Compass](http://compass-style.org/)が古くなる時代が来るとは思っていませんでした。
: ※この辺は正直、好みですので僕もSass+Compass構成で書くこともあります。
- Javascriptは書きたくない
: [CoffeeScript](http://coffeescript.org/)を使用する。
: しっかり型付けを意識したいときは[TypeScript](http://www.typescriptlang.org/)を使ってますが、イライラが凄いです、記法がだるい。
: mizuchi先生、[TypedCoffeeScript](https://github.com/mizchi/TypedCoffeeScript)はよ！
- 車輪の再発明はしたくない
: [Bower](http://bower.io/)+[npm](https://www.npmjs.com/)+[Browserify](http://browserify.org/)で既存のライブラリを最大限使用する。
- コンパイル作業は楽にやりたい
: [Grunt](http://gruntjs.com/) is God
: [Gulp](http://gulpjs.com/)は別に嫌いではないですが、後で何やってるのかをぱっと見る時にツリーで閉じれるのでGruntfileの方が好きなんです。

### フォルダ構成
~~~
ProjectRoot
├── dist(コンパイル先)
│      ├──css(main.css)
│      ├──js(common.js)
│      ├──img
│      ├──lib(vender)
│      └──index.html
├── src(コンパイル前)
│      ├──coffee(common.coffee)
│      ├──jade(index.jade)
│      ├──img
│      └──styl(main.styl)
├─.bowerrc
├─bower.json
├─bower_components(bower installのインストール先)
├─package.json
└─node_modules(npm installのインストール先)
└─s3auth.json(AWSS3にファイルを送るための設定)
~~~

### npmの設定

#### package.jsonの生成

{% highlight bash shell scripts linenos %}
$ npm init
{% endhighlight %}

#### 必要なモジュールをインストール

- おそらくglobalに突っ込んでいる事が多いが、他の人が使うことも考えて
: 明示的に何を使っているのか示しておいた方がいい

{% highlight bash shell scripts linenos %}
$ npm install grunt install --save-dev
$ npm install bower install --save-dev
$ npm install browserify --save-dev
{% endhighlight %}


### bowerの設定

#### .bowerrcを作っておく
{% highlight json linenos %}
{
  "directory": "bower_components"
}
{% endhighlight %}

- [公式ドキュメント](http://bower.io/docs/config/)が正直一番詳しい
- Windowsだと_bowerrcが優先される……？(未調査)

#### bower.jsonの生成

{% highlight bash shell scripts linenos %}
$ bower init
{% endhighlight %}

- 色々コマンドラインで聞かれるので必要な情報を入力していく

#### bower へのパッケージ追加

{% highlight bash shell scripts linenos %}
$ bower install [packagename] --save //本番でも使用
$ bower install [packagename] --save-dev //開発版だけ使用
{% endhighlight %}

- 基本的な使用方法はnpmと殆ど同じですね。

### s3auth.jsonを書く
{% highlight json %}
{
  "accessKeyId": "Your Access Key",
  "secretAccessKey": "Your Secret Access key",
  "region": "ap-northeast-1",//東京
  "bucket": "Your Hosting Bucket"
}
{% endhighlight %}

- 必要に応じて書き換えて下さい。

### Gruntfile.coffee

- 後で解説書く

{% highlight coffee linenos %}
module.exports = (grunt) ->
  'use strict'
  grunt
    .initConfig
      pkg: grunt
        .file
        .readJSON "package.json"
      s3auth: grunt
        .file
        .readJSON 's3auth.json'
      aws_s3:
        options:
          accessKeyId: '<%= s3auth.accessKeyId %>'
          secretAccessKey: '<%= s3auth.secretAccessKey %>'
          region: '<%= s3auth.region %>'
          access: '<%= s3auth.access %>'
        production:
          options:
            bucket: '<%= s3auth.bucket %>'
            differential: true
          files: [
            expand: true
            cwd: 'dist/'
            src: '**'
            dest: ''
          ]
        clean_production:
          options:
            bucket: '<%= s3auth.bucket %>'
            debug: true
          files: [
            dest : '/'
            action: 'delete'
          ]
      browserify:
        dist:
          files:
            'dist/js/main.js': ['src/coffee/main.coffee']
          options:
            transform: [
              'coffeeify'
              'debowerify'
            ]
            browserifyOptions:
              extensions: ['.coffee']
      stylus:
        app:
          files:
            expand : true
            cwd : 'src/styl'
            src : ['**/*.styl', '*.styl']
            dest : 'dist/css'
            ext : '.css'
      jade:
        compile:
          options:
            pretty: true
          files: [
            expand: true
            cwd: "src/jade"
            src: [
              "sp/*.jade"
              "*.jade"
              "!mixin/*.jade"
            ]
            dest: "dist"
            ext: ".html"
          ]
      bower:
        install:
          options:
            targetDir: "dist/lib"
            layout: 'byType'
            install: true
            cleanTargetDir: true
            cleanBowerDir: false
      sitemap:
        dist:
          siteRoot: 'dist/'
          homepage: 'http://www.hogehoge.com/'
      connect:
        server:
          options:
            port: 3000
            hostname: "*"
            base: 'dist'
            livereload: 35729
      esteWatch:
        options:
          dirs: [
            "src/coffee/**"
            "src/jade/**"
            "src/sass/**"
            "dist/**"
          ]
          livereload:
            enabled: true
            extensions: ['js', 'html', 'css']
            port: 35729
        "coffee": (path) ->
          ["newer:browserify"]
        "jade": (path) ->
          ["jade"]
        "sass": (path) ->
          ["compass"]
  grunt
    .loadNpmTasks 'grunt-contrib-jade'
  grunt
    .loadNpmTasks 'grunt-contrib-connect'
  grunt
    .loadNpmTasks 'grunt-contrib-stylus'
  grunt
    .loadNpmTasks 'grunt-este-watch'
  grunt
    .loadNpmTasks 'grunt-newer'
  grunt
    .loadNpmTasks 'grunt-bower-task'
  grunt
    .loadNpmTasks 'grunt-sitemap'
  grunt
    .loadNpmTasks 'grunt-browserify'
  grunt
    .loadNpmTasks 'grunt-aws-s3'
  grunt.registerTask 'make', ['bower', 'newer:browserify', 'jade', 'stylus', 'sitemap']
  grunt.registerTask 'default', ['make', 'connect', 'esteWatch']
  grunt.registerTask 'publish', ['aws_s3:clean_production', 'aws_s3:production']
{% endhighlight %}
