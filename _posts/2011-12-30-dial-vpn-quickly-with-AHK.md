---
layout: post
title: AHK速拨VPN
---

{{page.title}}
==============

<p class="meta">30 Dec 2011 - NanJing</p>

这两天在研究AHK，平时在PC上做的重复工作太多了。比如拨VPN这一项（用Proxy SwitchySharp管Chrome的SSH总是感觉莫名的lag，失去耐心了），点来点去累死人。想写个脚本自动拨/断开VPN，一开始用的是vpn连接的快捷方式，模拟键盘点击。这样就不知道怎么断开了=。=断开界面的快捷方式找不到也。所以就想用cmd命令连吧，在XP下的时候发现连了vpn就有个名字带“vpn”的进程，想检测其存在就断开，回到Windows 7下米有了，和vpn有关的那个Wmi进程不管连不连都存在，第一次写bat太不熟悉了，后来想到一个办法：

    rasdial vps /disconnect|findstr 没有连接 && rasdial vps usrname passwd

先尝试断开连接，如果返回没有连接就运行拨号命令，否则就断开。
然后在AHK script里加上 `#v:: run “~\vps.bat”` 只要按下win+v就可以自动拨号/断开了。

update1:  
忘记说了，通常我把批处理文件和快捷方式放在一个文件夹里，要运行文件夹里的批处理文件的话需要把这个文件夹加入Path环境变量。如何加自行Google。

update2:  
注意bat文件以ANSI方式编码。

update3:  
想玩Github了就把这个只有一行的脚本传上去了，就当HelloWorld吧，更新了一下，把控制台的提示信息改得合理了一些（喂快滚去复习离散数学啊！