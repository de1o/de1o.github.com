---
layout: post
title: 在 Windows 上安装 jekyll 环境
---

{{page.title}}
==============

<p class="meta">15 Feb 2012 - NanJing</p>

在公司的机器上又配置了一遍jekyll。这种东西久了不操作就忘，还是把详细的步骤记下来比较好。

1.  安装Ruby，在Windows上我们使用RubyInstaller这个工具（主站疑似被墙）。[上次][lasttime]装ruby192的时候遇到控制台编码问题（大概是jekyll的[问题][encodeissue]）。这次先选择了ruby187(?)，结果遇到 `` ERROR: While executing gem ... (TypeError)instance of Date needs to have method `marshal_load' `` 。搜了下没找到好办法，还是卸了187装回192。

2.  此时还不能通过gem安装jekyll。会遇到 `Failed to build gem native extension` 的错误。于是我们需要[RubyInstaller DevKit][rbdkdl]。下载好自解压文件后按[Wiki][dkwiki]里的步骤配置。

3.  这个时候就能通过 `gem install jekyll` 来安装jekyll了。需要提醒的是，gem的官方源被墙干扰，建议换taobao源。执行 `gem source http://ruby.taobao.org` 即可。安装可能会失败，这是由于需要安装其他的依赖包，按提示install就行。

4.  安装好进入本地github page repo所在目录。执行 `jekyll --server` 开启服务器。这时会报编码问题。建议不要使用Windows的cmd，gitbash是个不错的选择，前提是你使用的为安装版git而非Portable版。Portable版无法拿到Windows环境变量（也许可以通过配置profile达到效果但我对路径的处理有点迷惑，暂时不考虑了），无法在其中使用Ruby相关命令。假设你已经在使用安装版的git，打开 `$YOUR_GIT_DIR/etc/profile` 在最后加入
        export LC_ALL=en_US.UTF-8
        export LANG=en_US.UTF-8
    即可。
    
5.  建议把md解释器由默认的Maruku换成RDiscount。首先 `gem install rdiscount` ，然后在站点的_config.yml 里把markdown后面改为 rdiscount。效率高而且对中文字符的支持好。（忘记在哪里看到的了，如果不是请批评）

P.S. 一直不知道怎么在gitbash里粘贴，每次输入token，repo link的时候就蛋疼得不行。今天偶然发现用编辑区的insert搞定XDD

[encodeissue]: https://github.com/imathis/octopress/issues/232
[lasttime]: /fix-the-ruby192-encoding-error-while-reading-yaml/
[rbdkdl]: https://github.com/oneclick/rubyinstaller/downloads/
[dkwiki]: https://github.com/oneclick/rubyinstaller/wiki/development-kit