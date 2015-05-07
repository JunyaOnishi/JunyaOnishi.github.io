---
layout: post
title: ".gitignoreをpecoで便利に作りたかった。"
date: 2015-04-22 10:12:50
categories: lua
---
### コード

{% highlight lua linenos %}
alias {
  pgi = function ()
    local str = nyagos.eval("gibo -l | peco")
    local val
    if str then
      val = errCheck(toSpace(str))
    end
    if val then
      nyagos.exec("gibo "..toSpace(str).." > .gitignore")
      print("gibo make .gitignore of '"..val.."' .")
    else
      print("gibo fail make .gitignore .")
    end
  end
}
function toSpace(str)
  return string.gsub(str, '[\n|\r\n]', ' ')
end
function errCheck(str)
  if string.find(str, '[===]') then
    return nil
  elseif str == '' then -- escape key 対策
    return nil
  else
    return str
  end
end
{% endhighlight %}

### 使い方

{% highlight bash shell scripts %}
C:> pgi
{% endhighlight %}

pecoでgibo -lが選択できるようになるので、ignoreしたいものを選択する。
pecoは機能的にCtrl+Spaceで複数検索ができるのでそれにも対応している。
便利な世の中になったものです。

