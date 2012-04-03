---
layout: post
title: debian中设置vi显示utf-8编码内容（Windows下远程连接版）
---

{{page.title}}
==============

<p class="meta">3 Apr 2012 - Nanjing</p>

装好了markdoc后，用git把Windows机器上的项目提交上去，再在Linux上pull（还不知道怎么直接从仓库里得知最新版本的文件T-T），然后发现带中文字的文件显示乱码。当然vi也无法输入中文，输入会变成一个个的点点`...`。Google折腾了一下，基本理解了一些概念。

1. Linux上的系统全局编码在`/etc/profile`里通过`export LANG="zh_CN.UTF-8"`也就是LANG变量来指定。

2. 而要显示和输入中文，系统必须加入中文字库，首先通过`apt-get install zhcon`安装zhcon软件包。再 `less /usr/share/i18n/SUPPORTED`查看支持的字库，找到zh_CN字样就说明有中文库。

3. 编辑`/etc/locale.gen`，把`zh_CN.GBK GBK` `zh_CN.UTF-8 UTF-8`所在行的注释取消。（我顺便把其他编码，包括TW和HK的BIG5也加入进来了）

4. 执行脚本`usr/sbin/locale-gen`来生成具有中文输入库的支持包。

5. 编辑`/etc/profile`，添加`export LANG="zh_CN.utf-8`

    ！重新登录用户以使`/etc/profile`执行生效。
    
    到这里debian就有了显示和输入中文的能力了。但能否正确显示还有依赖于编码。
    
    debian的全局编码被我们设定为中文utf-8。这时用vi编辑内容为utf-8编码的文件**理论上**显示是正常的（下面会解释为什么是理论上）。但是，如果用vi去编辑GBK编码的文件又会显示为乱码，内容为GBK的文件还是有可能接触到的，所以我们需要设置vi使它能同时识别两种编码。

6. vi的编码相关可以在`~/.vimrc`中设置（这样的设置只对当前用户生效？）其中和编码相关的值有：1) encoding: 默认根据locale设置，也就是说，上面`/etc/profile`里设定的LANG值。只能在vim启动的时候设置一次，用户手册只建议在`~/.vimrc`里改变它的值； 2) termencoding: terminal的编码，也就是用于屏幕显示的编码； 3) fileencoding: 文件的编码，也就是vi会按这里面声明的编码去解码文件的内容。可以设置多个值，按顺序尝试。这正是我们需要的，所以在.vimrc里添加`set fencs=utf-8,gbk`即可。

    如果不设置termencoding和fileencoding，则继承encoding的设置。上面locale的LANG设置为utf-8后，vi本应该可以正常解码内容为utf-8的文件了。但是会发现不仅是文本内容，连vi的菜单内容也变成了乱码。这是因为Windows上的cmd默认代码页是cp936的GBK编码。它会把送到stdout的内容用GBK去解码。而实际上我们送来的是utf-8(termencoding setting)。
    
    在Windows cmd的代码页不修改的前提下（改这个可能引发很多问题，多一事不如少一事），推荐使用PuTTY来连接远程的Linux机器。而在PuTTY里可以方便地设置显示编码，具体在`Window->Translation->Romote character set`中选UTF-8就可以了。这样utf-8编码的文字都可以正确地在PuTTY的文本界面显示。当然如果你有文件名是GBK的那肯定是显示不了的，可以用iconv工具转码。
    
参考（抄袭）资料：

*  [debian显示中文][1]

*  [linux下编码转换][2]

*  [vim中的编码问题][3]

*  [Linux中通过locale来设置字符集][4]

*  [VIM解决中文编码问题][5]

[1]: http://blog.donews.com/vanfan/archive/2009/10/26/1568546.aspx
[2]: http://blog.csdn.net/zhuying_linux/article/details/7084518
[3]: http://hi.baidu.com/lee_eva/blog/item/89ddc1120122f88b6438db51.html
[4]: http://www.linuxsky.org/doc/newbie/200707/84.html
[5]: http://www.vimer.cn/2009/10/87.html