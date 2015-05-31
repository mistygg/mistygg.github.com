---
layout: post
title:  Linux 给普通用户设置 sudo 权限
categories: linux
---

Linux的优势之一是权限控制，但是让新手困惑的也是权限控制。在安装完Ubuntu的时候默认的用户是具有sudo权限的，也就是可以从该用户切换到root用户，但是新建的用户一般是没有sodu权限的。这篇文章就简单的介绍如何给Ubuntu系统新建的用户添加sudo权限。

## 创建用户 ##

Ubuntu创建用户很简单，使用```useradd```或者```adduser```都可以。它们最大的区别就是前者会创建家目录，后者不会。

```bash
useradd www
passwd www
# input passwd
```

## 切换用户，发现问题 ##

```bash
#从root切换到www，可以切换
su www
#从www切换到www，出现问题
sudo su
#Sorry, try again.
```

## 解决问题 ##

退出当前用户，直接回到之前的用户

```bash
exit 
```

选取root用户，给文件``` /etc/sudoers```赋予写的权限。（因为默认的权限是只读）

```bash
chmod u+w /etc/sudoers
```

修改```/etc/sudoers```文件，将```www```用户加入其中

```
# 先备份一下sudoers文件哦。
vim /etc/sudoers
# 加入如下内容
www     ALL=(ALL:ALL) ALL
# 保存退出，然后还原sudoers文件的权限
chmod u-w /etc/sudoers
```

## 总结 ##

就这么简单，你的www用户就可以直接用于sudo权限了。


