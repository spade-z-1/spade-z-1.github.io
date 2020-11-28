---
title: 密码爆破
author: Spade-Z
top: false
cover: false
toc: true
mathjax: false
img: 'https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/featureimages26.jpg'
coverImg: 'https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/featureimages26.jpg'
summary: kali使用hydra工具进行ssh服务器密码爆破
tags:
  - kail
  - Hydra的使用
  - 渗透测试
categories:
  - 软件工具
abbrlink: 74455b60
date: 2020-06-06 16:37:50
password:
---

hydra破解工具支持多种协议的登录密码，可以添加新组件，使用方便灵活。hydra可在Linux、Windows和OS X中使用。hydra可以用来破解很多种服务，包括IMAP,HTTP,SMB,VNC,MS-SQL,MySQL,SMTP等等。

# hydra的主要选项
<p>-R&nbsp;&nbsp;&nbsp;修复之前使用的aborted/crashed session</p>

<p>-S&nbsp;&nbsp;&nbsp;执行SSL(Secure Socket Layer)连接</p>

<p>-s&nbsp;&nbsp;&nbsp;Port 使用非默认服务器端口而是其他端口时，指定其端口</p>

<p>-l&nbsp;&nbsp;&nbsp;Login 已经获取登录ID的情况下输入登录ID</p>

<p>-L&nbsp;&nbsp;&nbsp;FILE 未获取登录ID情况下指定用于暴力破解的文件（需要指出全路径）</p>

<p>-p&nbsp;&nbsp;&nbsp;Pass 已经获取登录密码的情况下输入登录密码</p>

<p>-P&nbsp;&nbsp;&nbsp;FILE 未获取登录密码的情况下指定用于暴力破解的文件（需要指出全路径）</p>

<p>-x&nbsp;&nbsp;&nbsp;MIN:MAX:CHARSET 暴力破解时不指定文件，而生可以满足指定字符集和最短、最长长度条件的密码来尝试暴力破解</p>

<p>-C&nbsp;&nbsp;&nbsp;FILE 用于指定由冒号区分形式的暴力破解专用文件，即ID:Password形式</p>

<p>-M&nbsp;&nbsp;&nbsp;FILE指定实施并列攻击的文件服务器的目录文件</p>

<p>-o&nbsp;&nbsp;&nbsp;FILE以STDOUT的形式输出结果值</p>

<p>-f&nbsp;&nbsp;&nbsp;查找到第一个可以使用的ID和密码的时候停止破解</p>

<p>-t&nbsp;&nbsp;&nbsp;TASKS 指定并列连接数（默认值：16）</p>

<p>-w&nbsp;&nbsp;&nbsp;指定每个线程的回应时间（Waittime）(默认值：32秒)</p>

<p>-4/6&nbsp;&nbsp;&nbsp;指定IPv4/IPv6(默认值：IPv4)</p>

<p>-v/-V&nbsp;&nbsp;&nbsp;显示详细信息</p>

<p>-U&nbsp;&nbsp;&nbsp;查看服务器组件使用明细</p>



首先准备字典，可以选择密码在线生成工具，http://tools.mayter.cn/ 或者其它密码生成工具生成密码。
举例：在kali系统使用hydra工具进行服务器ssh方式登陆密码爆破。攻击前提是服务器的22端口需处于打开状态。
<pre>
<code class="language-bash hljs">hydra -l root -w 10 -P pwd.txt -t 10 -f 111.111.111.111 ssh</code>
</pre>

将ip换为需要攻击的服务器的ip即可。参数说明如下：

<p>-l root&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;指定爆破账号为root</p>

<p>-w 10&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;指定每个线程的回应时间为10S</p>

<p>-P pwd.txt&nbsp;&nbsp;指定密码字典为pwd.txt</p>

<p>-t 10&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;指定爆破线程为10个（部分服务器会限制并发数量，可设置为4线程）</p>

<p>-v&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;指定显示爆破过程</p>

<p>-f&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;查找到第一个可以使用的ID和密码的时候停止破解</p>
![](https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/Cryptoblast.png)
等待爆破结果，得到密码或者跑完字典就会结束进程。得到密码之后通过ssh登陆验证结果的正确性。

---
**欢迎在文章最后评论区【留言和讨论】，当然，欢迎点击文章最后的打赏按键，请博主一杯冰阔乐，笑～**
<escape>
<table>
  <tr>
    <td><img width="100" src="https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/alipay.bmp" ></td>    <td><img width="100" src="https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/wechat.bmp" ></td>    <td><img width="100" src="https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/zan.png" ></td>   
  </tr>
</table>
</escape>