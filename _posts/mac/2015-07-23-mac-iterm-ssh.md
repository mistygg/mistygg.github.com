---
layout: post
title:  Mac下iterm2支持ssh登陆信息保存
categories: [mac]
key: 1437804708
---

Mac下最强大的终端软件应该就是iterm2了（当然，不排除我见识浅陋	）。这边文章主要介绍一下如何让iterm2像SecureCRT一样能够记住ssh的登陆信息，不需要每一次都自己手动输入服务器的IP地址和用户的密码。是的，这篇文章的前提是Mac平台下。事实上这只是一个简单的思路，放到Linux或者Unix下都是适用的。

说明一下：这里需要用到Expect。Expect是一个免费的编程工具语言，用来实现自动和交互式任务进行通信，而无需人的干预。

## 创建Expect文件 ##

为每一台远程服务器创建一个Expect文件。我将下面这个文件命名为ssh111，并且将这个文件放到系统环境变量之中。为了简单，我直接将文件放到了```/usr/bin/```下面。

```shell

#!/usr/bin/expect -f

# 设定变量
set ip 192.168.0.111
set username  searchpcc
set password 123456
# 连接
spawn ssh $username@$ip
set timeout 30
# 处理第一次登陆时候的确认操作
expect {
        "(yes/no)?"
        {send "yes\n";exp_continue}
        "password:"
        {send "$password\n"}
}
interact

```

## 使用profile功能加载Expect文件 ##

利用 Command + o 快捷键打开Profiles功能，然后选择Edit Profiles...，接着新建自己的profiles。

![编辑profiles][profiles]

## 进行登录操作 ##

按下Command + o， 然后，选择你已经添加的profile，回车就可以自动登陆了。多个远程服务器设置添加多个Expect文件和多个profile就可以。

## 写在最后 ##

因为我目前所在的环境远程服务器众多，所以为了方便远程登陆服务器就网上找了一下并且结合自己的理解采用了这样一个方式进行ssh登陆记住用户信息的处理。事实上你根本不需要使用profiles，你直接在终端输入一下ssh111试试，是不是直接登陆上了服务器呢？profiles在这里只是能够记录更多的信息而已。由此可见，在Linux和Unix上只要支持Expect，是不是就可以了呢？

{% if site.model == 'pub' %}
[profiles]:   {{ site.pub.image }}profiles.png "编辑profiles"
{% else %}
[profiles]:   {{ site.dev.image }}profiles.png "编辑profiles"
{% endif %}



