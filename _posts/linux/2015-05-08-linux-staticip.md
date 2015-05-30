---
layout: post
title:  Linux 配置静态 IP
categories: linux
---

我现在的开发机器为```acer```，但是一般的开发环境还是在```Linux```之中，所以采用的是Windows的实体机 ```remote``` Linux的虚拟机的方式进行开发。由于，经常要使用```ssh```的方式进行虚拟机的远程操作，甚至有时候需要虚拟机在局域网内被访问，所以难免涉及到静态```IP```的配置。这篇文章主要记录的是```Ubuntu```和```CentOS```的静态IP的配置方式。

## Ubuntu配置静态IP ##

其实只需要修改三个文件即可。文件的目录和名称如下。

```yaml
# 文件1
/etc/network/interfaces
# 文件2
/etc/resolv.conf
# 文件3
/etc/resolvconf/resolv.conf.d/base
```

### 修改```/etc/network/interfaces```文件 ###

修改之前备份一下，cp /etc/network/interface /etc/network/interface.bak。

原有的文件内容如下：采用的是DHCP动态获取IP的方式。

```yaml
auto lo
iface lo inet loopback
auto eth0
iface eth0 inet dhcp
```

修改之后的文件内容如下：需要进行修改，添加IP地址，默认网关，子网掩码等信息。

```yaml
auto lo
iface lo inet loopback

auto eth0
#将dhcp获取IP的方式注释
#iface eth0 inet dhcp

#添加静IP
iface eth0 inet static
#IP东洲
address 192.168.6.130
#子网掩码
netmask 255.255.255.0
#默认网关
gateway 192.168.6.2
```

### 修改```/etc/resolv.conf```文件 ###

修改之前备份一下，cp /etc/resolv.conf /etc/resolv.conf.bak。

在文件之中加入如下内容。

```yaml
nameserver 192.168.6.2
```

### 修改```/etc/resolvconf/resolv.conf.d/base```文件 ###

其实，进行了上面两次修改，然后重启网络服务就可以使用静态IP了。但是，只做上面两处修改的话，重启系统之后不能正常使用。原因是在resolv.conf中配置的nameserver重启之后又失效了。这个时候应该修改base文件。在base文件之中加入如下内容:

```yaml
nameserver 192.168.6.2
```

### 重启系统 ###

```bash
sudo reboot
```

## CentOS配置静态IP ##

CentOS的静态IP的配置和Ubuntu的大同小异。事实上Redhat自己本身也有一个工具进行IP配置的。不信你在命令行输入setup试试。但是，这里我们还是和Ubuntu一样直接手动修改，不借助系统工具。修改以下三个文件。

```bash
/etc/sysconfig/network-scripts/ifcfg-eth0
/etc/sysconfig/network
/etc/resolv.conf
```

### 修改```/etc/sysconfig/network-scripts/ifcfg-eth0```文件 ###

修改之前备份一下。。cp /etc/sysconfig/network-scripts/ifcfg-eth0 /etc/sysconfig/network-scripts/ifcfg-eth0.bak

```yaml
DEVICE="eth0"
# 加入静态IP
BOOTPROTO="static"
# ip地址
IPADDR=192.168.6.129
# 子网掩码
NETMASK=255.255.255.0
# 默认网关
GATEWAY=192.168.129.2
ONBOOT="yes"
DNS1=192.168.129.2
```

### 修改```/etc/sysconfig/network```文件 ###

修改之前先备份一下。cp /etc/sysconfig/network /etc/sysconfig/network.bak

```yaml
NETWORKING=yes
HOSTNAME=localhost.localdomain
#默认网关
GATEWAY=192.168.129.2
```

### 修改```/etc/resolv.conf```文件 ###

修改之前同样备份一下。cp /etc/resolv.conf /etc/resolv.conf.bak

```yaml
nameserver 192.168.6.2
```

### 重启系统 ###

```bash
sudo reboot
```

## 总结 ##

Ubuntu和CentOS的静态IP地址的配置大同小异。虽然一个是Debain系列一个是RedHat系列，但是毕竟都是Linux嘛。一般的步骤是：先修改ip地址，子网掩码，默认网关；然后添加DNS；最后重启系统。这里注意一下，有时候只是单纯的重启网络服务是不成功的，注意所有的配置文件修改之前务必备份。




