---
layout: post
title:  使用Python来提高工作效率
---

{{page.title}}
==============

<p class="meta">4 Mar 2012 - NanJing</p>

上星期接到个活，要对比某程序GET的图片，浏览器GET的图片，HTML代码里的`<img>`标签src属性里的图片有哪些差异。也即比较哪些图片某程序没GET而浏览器GET了等等。测试要访问10个网站（譬如四大门户），每个网站独立重复五次，三个部分两两对比就是六次，也就是说要对比两个文件每两行是否一样300次（顺便，HTML里的`<img>`好像还得`ctrl-F`然后不断地下一个是吧）。单说网易，首页上`<img>`标签有100+个，链接的文件名充斥着hfsuijfhgdo.jpg 这样的随机串，要用肉眼一一去对比简直是<del>bi</del>自虐。

Whatever，既然我学过python这把人挡杀人，佛挡杀佛的[电锯][electricSaw]，现在就拿出来练练。首先，某程序GET过的图片从数据库里导出成app.txt，这里用的是toad的功能（当然python也能和数据库交互，也确实很简单，但当时不想把战线拉太长就直接用toad了）。浏览器的GET记录用HTTP Analyze导出成brow.txt。这里还有一个累人的点是每次浏览器访问之前必须把缓存清空，以免造成有缓存而不GET的情况（好吧其实这里也可以用脚本搞定）。而HTML源码里的`<img>`如何全获取出来是需要解决的问题。

[electricSaw]: http://coolshell.cn/articles/6639.html

一开始我尝试使用SGMLParser。参照Dive into Python里的相关内容，定义了starttag_img函数。在`qq.com`测试的时候很棒，抓取出来的链接和ctrl-F找到的`<img>`一样多，跑到`www.sohu.com`的时候傻眼了，少了若干条。搜索发现是SGMLParser里的正则匹配没有考虑中文，所以当遇到`<img alt=中文 src='somelink'>`这样，alt属性的内容不加引号而src属性又在alt之后的不规范写法时，SGMLParser就傻眼了。至于HTMLParser对中文的支持貌似更差，压根就没跑起来。另外，某些写在js里else语句之后的`<img>`没有被SGMLParser纳入，难道其中还带运行js然后判断某行是否被动态写入了的功能（浏览器的GET记录里也没有这张图片）？不能完全地搜索出HTML里所有的`<img>`会给后期的对比增加工作量。所以我决定自己写一个正则来匹配。诸多周折，最终用来获取`<img>`的脚本不是完美的，但对于此项工作来说，perfect enough!

由此得到了三个文件，文件的每一行是一个链接，找出两两之间独有的链接即可。数据量很小，直接用`if line is in list`解决。