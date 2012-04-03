---
layout: post
title: 解决Apache不能启动的问题
---

{{page.title}}
==============

<p class="meta">30 Dec 2011 - Nanjing</p>

淫长在装Apache的时候遇到了问题，本来以为是端口什么的结果不是，最后查error.log发现: `(OS 10022)提供了一个无效的参数。 : Child 2848: setup_inherited_listeners(), WSASocket failed to open the inherited socket.`

搜索了下，取消了查询LMHOSTS解决问题。刚又搜了下原理发现是winsock接口上的一些blabla。所以Apache启动排错可以：

1. 检查端口
2. 检查httpd.conf配置
3. 取消查询LMHOSTS
4. netsh winsock reset 修复winsock
