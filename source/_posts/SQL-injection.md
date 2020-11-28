---
title: SQL注入
author: Spade-Z
top: false
cover: false
toc: true
mathjax: false
img: https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/featureimages31.jpg
coverImg: https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/featureimages31.jpg
summary: >-
  SQL注入攻击是通过将恶意的SQL查询或添加语句插入到应用的输入参数中，再在后台SQL服务器上解析执行进行的攻击，它目前是黑客对数据库进行攻击的最常用的手段之一。本课程将带你从介绍web应用运行原理开始，一步一步理解SQL注入的由来，原理和攻击方式。
tags:
  - SQL注入
  - web漏洞
  - web防护
categories:
  - web安全
abbrlink: e73517db
date: 2020-03-22 19:25:14
password:
---

# 一、sql注入原理

注入攻击的本质就是把用户输入的数据当作代码来执行。所以注入攻击有两个必要条件

- 1.用户能够控制的输入。

- 2.原本程序要执行的代码，拼接了用户输入的数据。

# 二、sql注入分类

- 按照请求方法可以分为：GET请求、POST请求

- 按照参数类型可以分为：数字型、字符型

- 按照数据返回结果分为：回显、报错、盲注

- 盲注又分为：布尔盲注、延时盲注

# 三、sql注入测试方法

## 一般测试语句：

<table>
    <tr>
     <td style="text-align: center;">or 1=1 --+</td>
     <td style="text-align: center;">'or 1=1 --+</td>
     <td style="text-align: center;">"or 1=1 --+</td>
     <td style="text-align: center;">)or 1=1 --+ </td>
    </tr> <tr>
     <td style="text-align: center;">')or 1=1 --+</td>
     <td style="text-align: center;">")or 1=1--+</td>
     <td style="text-align: center;">"))or 1=1 --+</td>
    </tr>
</table>

- url编码后为 %23 ，可以用 --+ 替换

## 常用测试函数：

<table>
    <tr>
       <td style="text-align: center;">函数名<td>
       <td style="text-align: center;">作用<td>
    </tr><tr>
       <td style="text-align: center;">version()<td>  
       <td style="text-align: center;">数据库版本<td>
    </tr><tr>
       <td style="text-align: center;">user()<td>
       <td style="text-align: center;">数据库用户名<td>
    </tr><tr>
       <td style="text-align: center;">database()<td>
       <td style="text-align: center;">数据库名<td>
    </tr><tr>
       <td style="text-align: center;">@@datadir()<td>
       <td style="text-align: center;">数据库路径<td>
    </tr><tr>
       <td style="text-align: center;">@@version_compile_os<td>
       <td style="text-align: center;">操作系统版本<td>
    </tr>
</table>

 ## 测试流程：
 
 >这里是在本地搭建的一个 [sqli](https://github.com/Audi-1/sqli-labs)的靶场，用来自己做练习，感觉还不错。	

### 1.检测sql注入类型
---
![](https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/SQL-injection0.jpg)

>直接在url处添加 单引号 发现网站报错、说明sql语句出错，就可能存在注入

### 2.闭合sql语句

一般有两种方法：

- a.使用 # 号，把本行 # 号后面的内容注释调，这样就可以避免sql语句出错，使我们构造的 payload 可以正确执行

- b.根据sql语句，用符号进行闭合

![](https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/SQL-injection1.jpg)

>这里就直接用 # 进行注释了，可以看到网站的页面回复了正常	

### 3.探测字段列数

假设列数为 4 进行测试，页面出错，说明小于 4 列	
![](https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/SQL-injection2.png)

然后列数为 3 进行测试，页面正常，说明存在 3 列
![](https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/SQL-injection3.jpg)

### 4.查看显示位，进行测试

![](https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/SQL-injection4.jpg)
>可以看到数据库的版本信息，说明存在sql注入漏洞
![](https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/SQL-injection5.jpg)

# 四、sql注入防护技巧

- 数据与代码分离

- 对用户输入的数据进行严格过滤

- 对特殊字符进行转义

- 使用预编译语句

- 使用安全函数

- 检查数据类型

---
**欢迎在文章最后评论区【留言和讨论】，当然，欢迎点击文章最后的打赏按键，请博主一杯冰阔乐，笑～**
<escape>
<table>
  <tr>
    <td><img width="100" src="https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/alipay.bmp" ></td>    <td><img width="100" src="https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/wechat.bmp" ></td>    <td><img width="100" src="https://cdn.jsdelivr.net/gh//SpadeZ-A/cdn/img/zan.png" ></td>   
  </tr>
</table>
</escape>