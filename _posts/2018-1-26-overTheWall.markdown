---
layout:     post
title:      "ubuntu下翻墙总结"
subtitle:   "其余平台供参考"
date:       2018-1-26 12:00:00
author:     "Dugreen"
header-img: "img/artical-bgs/back３.jpg"
catalog: true
tags:
     - linux
---

> 记得第一次翻墙是去年做安卓的时候，需要到谷歌的安卓官网去查询开发文档。当时用的一些免费的vpn,也是在windoes平台上，后来在17年七月出现了“查水表被喝茶”的事件后，就很少翻墙了。可是总感觉Bing没有Google的搜索好用，又开始了折腾翻墙的门路。

### 通过vpn实现翻墙

这个在ubuntu上没有尝试过，因为目前很多的vpn平台都倒了，而且服务付费。

### 通过shadowsocks实现翻墙

俗称ss的翻墙操作。这个还是比较稳的。大概就一下几个步骤。

```
$ sudo apt install shadowsocks
$ vim gui-config.json
# 然后配置这个json文件
# 一般debian系列的长这个样子
du@du:~/Demo/ss$ cat gui-config.json
{
    "method": "rc4-md5",
    "password": "943261",
    "remarks": "du",
    "server": "us01.ipip.pm",
    "server_port": 13555,
    "local_address":"127.0.0.1",
    "localPort": 1080,
    "shareOverLan": true
}
# redhat系列的长这个样子
du@du:~/Demo/ss$ cat gui-config.json
{
    "configs": [
        {
            "method": "aes-256-cfb",
            "password": "u1rRWTssNv0p",
            "remarks": "du",
            "server": "198.199.101.152",
            "server_port": 8388
        }
    ],
    "localPort": 1080,
    "shareOverLan": true
}

# 运行ss
$ sslocal -c ~/pathTo/gui-config.json ('_'改成自己的路径)
```

之后在系统的网络里设置全局代理：

![](http://linux.it.net.cn/uploads/allimg/160327/1-16032H213302P.jpg)

然后就可以输入www.google.com来试验一下了。

给大家分享一个获取免费ss账号的网址 [ishadowx](https://global.ishadowx.net) 找免费试用。

### 通过Tor来实现翻墙

这个翻墙操作特别的骚气（哈哈哈）。洋葱网络我就不做任何介绍了，直接放安装教程。

到[官网](https://www.torproject.org/)找到适合自己平台的压缩包文件（我这里是linux64位的压缩包）。如果连这个都找不到，那我建议还是省省心去移动端找个啥付费的vpn吧！

```
$ xz -d tor-browser-linux64-7.5_en-US.tar.xz
$ tar -xvf tor-browser-linux64-7.5_en-US.tar
```

之后到解压后的目录里，点击启动图标。

之后的操作就是配置网桥。选择meek-amazon。然后就没了。。。我记得我去年在windows上有一堆窗口弹出来要配置的，可能是Tor优化了吧！

![](img/artical-includes/tor.png)

### 不能叫做翻墙的操作

通过ipv6来访问google youtube 等网站。这个操作其实目前来看比较局限，大部分的网络运营商并没有提供ipv6的服务。所以这个操作只能在有运营商开通ipv6访问的网络上进行。（至今我也没有尝试成功过）

建议大家先到这个网站测试一下自己的网络运营商有没有开通ipv6服务。http://test-ipv6.com/

如果测试成功，下载这个仓库(https://github.com/lennylxx/ipv6-hosts)里的hosts文件，然后对自己本地的hosts文件(/etc/hosts)进行替换（建议原来的hosts文件备份）。

其实直接对hosts文件的复制并非是正确的操作（直接复制的hosts文件有很多IP地址是无效的状态）。建议大家到这个页面正确的处理一下hosts文件。https://github.com/lennylxx/ipv6-hosts/wiki/Do-It-Yourself

然后在自己的浏览器上安装https everywhere这个插件。

之后就可以愉快的玩耍了。

这就是我这两天翻墙操作的总结吧！可能还有一种ssh+Firefox+代理进行翻墙的操作，没有尝试做。因为自己平时都是用Chromeium.
