---
layout: post
title: 让eclipse支持retina display
---
偶然看到eclipse更新了大版本，做为资深码友，怎么能不顺手更新一个呢？于是乎，10分钟的等待后，悲剧发生了……我了个擦，居然不支持retina显示器，我的rMBP表示压力很大啊！难不成要对着一片毛边编程？！

不能，不能这样，于是开始Google，果然看到有人已经给官方提了这样一个BUG，下面的comment里，有个大神给了一个办法解决了这个问题，亲测非常给力啊。

但是呢，貌似没有找到中文版本，为了方便用中文的朋友也能顺利的用上rBMP+eclipse，于是乎，我写了这篇博客。

其实很简单，只需要打开eclipse.app这个目录，在Contents/Info.plist里添加一对键值就好了：

{% highlight xml %}
<key>NSHighResolutionCapable</key>
<true/>
{% endhighlight %}

把这段代码放到Info.plist文件的最后，</dict></plist>前面。修改后这个文件里后五行是这样的：

{% highlight xml %}
   		<key>NSHighResolutionCapable</key>
		<true/>
	</dict>
</plist>
{% endhighlight %}

这个时候，你再次打开eclipse是不会生效的，因为MacOS会缓存Info.plist里的信息，建议是复制一份这个Eclipse.app，然后把原来的删除掉，再把这个复制到改回原来的名字。此时再启动就应该是OK了！

好了，enjoy it!
