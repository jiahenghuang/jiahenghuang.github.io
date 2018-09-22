---
title: 在腾讯云上搭建ss服务器
mathjax: false
date: 2018-09-22 16:22:20
tags:
- ss服务器
- 服务器
- shadowsocks
categories:
- 科学上网
---

​	以前不明白ss服务器跟shadowsock有什么关系，查到的教程都是搭建ss服务器都会安装shadowsocks，猜测搭建ss服务器就是就是shadowsocks的部署过程。

# 1.服务端配置

- 在腾讯云上安装shadowsocks

```shell
$ pip install shadowsocks
```

- 创建shadowsock配置文件

```shell
$ vim shadowsocks.json
#把下面的内容添加进去
{
  "server": "0.0.0.0", #参考链接中有人试过别的地址，说是不行，我也没试其他的，能用就行
  "server_port": 8388, #这个端口应该无所谓，我试过改成其他ip，开启全局代理，仍然可以打开网页
  "password": "abcdefg",
  "method": "aes-256-cfb" #加密方式挺多的，如cr4-md5
}
```

- 在腾讯云服务器上启动ss服务

```shell
#-c是配置文件的路径，在终端输入ssserver可以查看各个参数的描述
$ ssserver -c shadowsocks.json -d start
```

# 2.客户端配置

貌似在官网[shadowsocks](https://shadowsocks.org/en/index.html)下载适合不同系统的安装方式，这个我没试，用的是以前的客户端。

![](在腾讯云上搭建ss服务器/20180922170120.png)

服务器ip填你的腾讯云域名(或者公网ip?)就行，其他的跟前面在腾讯云服务器上的配置基本一样。

# 3.验证

​	**将shadowsocks设置成全局代理**，然后在浏览器中搜索本机ip地址，**如果显示为腾讯云服务器的ip地址说明ss服务器就算搭建成功了**。注意腾讯云上使用`ifconfig`命令可能跟你在浏览器中的ip地址不一样，那是因为服务器显示的是本地局域网中的ip地址，可以使用下面的命令查看腾讯云服务器的外网ip，这个ip才是跟你在浏览器中显示的一样的。

```shell
#注，下面这个如果很久没有响应的话就网上搜下，多试几个
$ curl icanhazip.com
```

参考链接：

- [搭建ss服务器和配置ss客户端](https://blog.csdn.net/hereis00/article/details/79552003)
- [用Linux命令行获取本机外网IP地址](https://blog.csdn.net/lakeheart879/article/details/78247894)

在linux上使用命令行走pac模式的技术**待续**

- [Ubuntu 16.04 安装ss客户端](https://blog.csdn.net/thor_w/article/details/79504804)
- [centos7 安装shadowsocks客户端](https://blog.csdn.net/guyan0319/article/details/72681796)
- [centos7 安装shadowsocks客户端](https://blog.csdn.net/guyan0319/article/details/72681796)
- [Centos + Shadowsocks客户端 + Privoxy实现外网访问](http://exp-blog.com/2018/07/04/pid-1591/)
- [VPS一键搭建PAC代理服务器脚本，比SS翻墙更简单](http://www.glseo.cc/post-103.html)