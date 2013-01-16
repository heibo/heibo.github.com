---
layout: post
title: JavaScript 面向对象编程
---

此处省略2万字(JavaScript背景知识)

JavaScript是面向对象的，这一点是没有疑问的，但是我看很多人并没有真正理解JavaScript的面向对象，具体的说，很多人用Java等Class Based的OO思想来想方设法的将JavaScript变成他们熟悉的Java。其实JavaScript采用的是另一种OO实现方式也就是基于原型(Prototype Based)的面向对象编程，借用了Self里的许多思想，关于这个的Prototype Based OO在JavaScript里的详细讲解可以参考 [这个](http://helephant.com/2008/08/17/how-javascript-objects-work/).

事实上Prototype Based虽然不是现在面向对象的主流，但是也绝对是非常重要的一种思想方法，甚至一些非常新非常酷的语言(E.g. [Io](http://iolanguage.com))仍然在沿用这样的技术。

下面就给出一个用JavaScript最原生的OO方法编写的简单实例，供参考：

{% highlight javascript %}
function _cb(func, context) {
    return function() {
        if (!context) {
           context = this;
        }
        func.apply(context, arguments);
    };
}


function Ajax(success, fail) {
    if (success) this.success = success;
    if (fail) this.fail = fail;
    
    this.xhr = new XMLHttpRequest();
}
Ajax.prototype = {
    init: function() {
        this.xhr.onreadystatechange = _cb(this.readystatechange, this);
    },
    encode: function(data) {
        result = "";
        for (key in data) {
            result += encodeURIComponent(key) + "=" + encodeURIComponent(data[key]) + "&";
        }
        return result;
    },
    get: function(url, data) {
        this.init();
        this.xhr.open("GET", url + "?" + this.encode(data));
        this.xhr.send(null);
    },
    post: function(url, data) {
        this.init();
        this.xhr.open("POST", url);
        this.xhr.send(this.encode(data));
    },
    readystatechange: function() {
        if (this.xhr.readyState == 4) {
            if (this.xhr.status == 200) {
                this.success(this.xhr.responseText);
            } else {
                this.fail();
            }
        }
    },
    success: function(data) {
        console.log("Ajax request finished!");
    },
    fail: function() {
        console.log("Ajax requeset failed!");
    }
};


function AjaxUpdater(element, success, fail) {
    this.element = element;
    if (success) this.updateSuccess = success;
    if (fail) this.fail = fail;
}
AjaxUpdater.prototype = new Ajax();
AjaxUpdater.prototype.success = function(data) {
    this.element.innerHTML = data;
    if (this.updateSuccess) this.updateSuccess();
};


window.addEventListener("load", function() {
    var updater = new AjaxUpdater(document.getElementById("test"));
    updater.get("xproto.js");
}, false);
{% endhighlight %}