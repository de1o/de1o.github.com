---
layout: post
title: Ruby1.9+版本在使用jekyll时的编码问题
---

{{page.title}}
==============

<p class="meta">6 Jan 2012 - NanJing</p>

clone了mojombo.github.com，用jekyll建_site的时候遇到错误：

`readyaml invalid byte sequence in gb2312 (argument error)` 

搜了一下找到[这个](https://github.com/mojombo/jekyll/issues/188)。里面推荐Windows下在cmd里用

    bc. set LC_ALL=en_US.UTF-8
    set LANG=en_US.UTF-8

貌似这种方法只能一次有效？另一个方法是

    chcp 65001

但以我用python的时候折腾cmd编码的经验来看，还是别改cmd的编码了，暂时无法全部理清这部分头绪。会出很多问题。还有人推荐回退到Ruby1.8的版本，这种方法我不太喜欢。

一般在Windows下玩Github Page都是用GitBash吧(本人才刚开始玩，轻拍)，本身就是模拟Linux Bash的MinGW肯定也有bash配置文件的，在 `Git\etc` 里找到profile文件，在文件的末尾添加

    export LC_ALL=en_US.UTF-8
    export LANG=en_US.UTF-8

这样每次启动Git Bash都会设置好编码，jekyll就可以成功翻译出站点文件。