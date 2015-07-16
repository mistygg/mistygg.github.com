---
layout: post
title:  Linux 配置 Subversion
categories: linux
key: 1437062940
---

目前最为流行的版本控制器有两种，一种是```Subversion```，另一种就是```Git```。当然，随着科学技术的发展，```Git```也已经有了覆盖```Subversion```之势。但是，在一些旧的项目或者是一些有```文化底蕴```的公司依旧使用```Subversion```进行版本控制。这篇文章将主要介绍如何在```Ubuntu```上安装和配置```Subversion```。


## 安装```Subversion``` ##

其实在```Ubuntu```上安装```Subversion```非常之简单，只需要下面一条命令即可。

```bash
sudo apt-get install subversion
```

## 创建```Subversion```项目 ##

主要的目的是：创建应用，创建```Subversion```目录，将应用导入到```Subversion```目录。

### 创建```Subversion```的工作目录 ###

```bash
#应用名称为test
sudo mkdir /var/www/test -p
#在test之中创建index.php测试文件
sudo touch index.php
```

### 创建```Subversion```的文件目录 ###

```
#在自己的家目录下面创建Subversion项目目录
sudo svnadmin create /home/pcc/test
```

### 导入项目到```Subversion```文件目录之中###

```bash
sudo svn import /var/www/test file:///home/pcc/test -m 'import project to svn'
```

## 配置```Subversion```权限 ##

主要的文件有如下几个：

```/home/pcc/test/conf/svnserve.conf```

```/home/pcc/test/conf/passwd```

```/home/pcc/test/conf/authz```

### 配置```svnserve.conf```文件 ###

```
#修改下面一行：
anon-access = read
#变更为：
anon-access = none #外来访客没有权限读取svn项目内容。
#修改下面一行：
authz-db = authz
# 变更为：
authz-db = authz #开启密码认证

#并且将以下三句的注释取消
auth-access = write
password-db = passwd
authz-db = authz
```

### 配置```passwd```文件 ###

```bash
# 添加三个用户，分别为test1，test2，test3
[users]
test1 = helloworld
test2 = helloworld
test3 = helloworld
```

### 修改```authz```文件 ###

```bash
#创建group，并且分配用户，当然事实上你可以创建多个，然后赋予不同的权限
[groups]
group1 = test1,test2,test3
[/]
@group1 = rw
*=r
```

### 重启```Subversion```服务 ###

```bash
svnserve -d -r /home/pcc/test
```

这里也额外的提到一下关闭（终止）```Subversion```。

```bash
ps -ef | grep svnserve
kill -9 Pid(你看到的Pid)
```

## 访问```Subversion``` ##

删除```/var/www/test```目录；

执行 ```sudo svn co file:///home/pcc/test```

然后输入用户名和密码即可。

## 总结 ##

### 踩过的坑 ###

遇到问题：使用  ```sudo svnadmin create /home/pcc/test```  创建```svn```应用的时候出现如下错误

```bash
svnadmin: warning: cannot set LC_CTYPE locale
svnadmin: warning: environment variable LANG is zh_CN.UTF-8
svnadmin: warning: please check that your locale name is correct
```

解决办法:

```bash
sudo vi /etc/profile
加入一行：
export LC_ALL=C
source /etc/profile
```

Subversion的基本的配置比较简单，但是实际上Subversion还可以支持http,https协议访问。如何配置http，https的方式访问，有时间我也会将相应的配置方法记入文章之中。



