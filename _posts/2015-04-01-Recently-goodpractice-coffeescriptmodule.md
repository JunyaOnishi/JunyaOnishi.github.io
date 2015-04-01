---
layout: post
title:  "最近の行儀の良いJavascriptをCoffeeScriptで書く"
date:   2015-04-01 18:30:50
categories: CoffeeScript
---
#### 最初に

- [WebModulePattern](https://github.com/uupaa/WebModule/wiki/WebModulePattern)

#### やり方1
- 入力
: 下記で何故、globalが取れるのかは[WebModuleIdiom](https://github.com/uupaa/WebModule/wiki/WebModuleIdiom)で確認して下さい
{% highlight coffee linenos %}
((global)->
  #エクスポーズをスマートにまとめてしまう方法
  "use strict"
  # Class
  ModuleName = ->
  # Header
  ModuleName["prototype"]["method"] = ModuleMethod
  # Implementation
  ModuleMethod = (Arg) ->
  # Exports
  if ("process" of global)
    module["exports"] = ModuleName
  global["ModuleName"] = ModuleName
)(
  (this or 0)
    .self or global
)
{% endhighlight %}

- 出力
{% highlight javascript linenos %}
(function(global) {
  "use strict";
  var ModuleMethod, ModuleName;
  ModuleName = function() {};
  ModuleName["prototype"]["method"] = ModuleMethod;
  ModuleMethod = function(Arg) {};
  if ("process" in global) {
    module["exports"] = ModuleName;
  }
  return global["ModuleName"] = ModuleName;
})((this || 0).self || global);
{% endhighlight %}

#### やり方2

(なんかはやってる、処理を上で直接的に書けるから仕様変更には強そう)

- 入力
{% highlight coffee linenos %}
((definition) ->
  # 定義する関数を引数にとる
  # ロードされた文脈に応じてエクスポート方法を変える
  if typeof exports is "object"
    # CommonJS
    module.exports = definition()
  else if typeof define is "function" and define.amd
    # RequireJS
    define definition
  else
    # <script>
    MyModule = definition()
)(->
  # 実際の定義を行う関数
  "use strict"
  MyModule = ->
  MyModule:: = {}
  MyModule()
)
{% endhighlight %}

- 出力
{% highlight javascript linenos %}
(function(definition) {
  var MyModule;
  if (typeof exports === "object") {
    return module.exports = definition();
  } else if (typeof define === "function" && define.amd) {
    return define(definition);
  } else {
    return MyModule = definition();
  }
})(function() {
  "use strict";
  var MyModule;
  MyModule = function() {};
  MyModule.prototype = {};
  return MyModule();
});
{% endhighlight %}
