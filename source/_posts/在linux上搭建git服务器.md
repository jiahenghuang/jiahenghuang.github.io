---
title: 在linux上搭建git服务器
date: 2018-08-23 08:55:52
tags: [git, 服务器, linux]
categories: git
---

## 在服务器上添加git用户

```shell
ls@ls:~$ id git
id: "git": no such user
ls@ls:~$ sudo adduser git
正在添加用户"git"...
正在添加新组"git" (1001)...
正在添加新用户"git" (1001) 到组"git"...
创建主目录"/home/git"...
正在从"/etc/skel"复制文件...
输入新的 UNIX 密码： 
重新输入新的 UNIX 密码： 
passwd：已成功更新密码
正在改变 git 的用户信息
请输入新值，或直接敲回车键以使用默认值
        全名 []: 
        房间号码 []: 
        工作电话 []: 
        家庭电话 []: 
        其它 []: 
这些信息是否正确？ [Y/n] y
ls@ls:~$ su git
密码： 
git@ls:/home/ls$ cd
git@ls:~$
```

## 配置服务器

```shell
git@ls:~$ mkdir .ssh && chmod 700 .ssh
git@ls:~$ touch .ssh/authorized_keys && chmod 600 .ssh/authorized_keys
```

## 将本地ssh公钥添加到git服务器

```shell
#如本地为windows的git bash
john@john MINGW64/$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCB007n/ww+ouN4gSLKssMxXnBOvf9LGt4L
ojG6rs6hPB09j9R/T17/x4lhJA0F3FR1rP6kYBRsWj2aThGw6HXLm9/5zytK6Ztg3RPKK+4k
Yjh6541NYsnEAZuXz0jTTyAUfrtU3Z5E003C4oxOj6H0rfIF1kKI9MAQLMdpGW1GYEIgS9Ez
Sdfd8AcCIicTDWbqLAcU4UpkaX8KyGlLwsNuuGztobF8m72ALC/nLF6JLtPofwFBlgc+myiv
O7TCUSBdLQlgMVOFq1I2uPWQOkOWQAHukEOmfjy2jctxSDBQ220ymjaNsHT4kgtZg2AYYgPq
dAv8JggJICUvax2T9va5 gsg-keypair
#将这个公钥复制到git服务器的authorized_keys中
git@ls:~$ cat > .ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCB007n/ww+ouN4gSLKssMxXnBOvf9LGt4L
ojG6rs6hPB09j9R/T17/x4lhJA0F3FR1rP6kYBRsWj2aThGw6HXLm9/5zytK6Ztg3RPKK+4k
Yjh6541NYsnEAZuXz0jTTyAUfrtU3Z5E003C4oxOj6H0rfIF1kKI9MAQLMdpGW1GYEIgS9Ez
Sdfd8AcCIicTDWbqLAcU4UpkaX8KyGlLwsNuuGztobF8m72ALC/nLF6JLtPofwFBlgc+myiv
O7TCUSBdLQlgMVOFq1I2uPWQOkOWQAHukEOmfjy2jctxSDBQ220ymjaNsHT4kgtZg2AYYgPq
dAv8JggJICUvax2T9va5 gsg-keypair
^C
```

**注：**这一步中`id_rsa.pub`中的公钥要改成自己本地的

## 在git服务器创建git仓库

```shell
git@ls:~$ git init --bare project.git
初始化空的 Git 仓库于 /home/git/project.git/
git@ls:~$ ls
project.git
#在本地克隆git服务器中新建的工程，由于事先将公钥添加到了git服务器authorized_keys中，除了第一次需要输入秘钥，第二次就不用输入了
john@john MINGW64/$ git clone git@192.168.159.128:project
Cloning into 'project'...
The authenticity of host '192.168.159.128 (192.168.159.128)' can't be established.
ECDSA key fingerprint is SHA256:gO0iw1+xe0Zg/nbdxTr9xu565+hQHfnXgNKIqpgh+sE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.159.128' (ECDSA) to the list of known hosts.
git@192.168.159.128's password:
warning: You appear to have cloned an empty repository.
#由于事先已经将公钥添加到了git服务器authorized_keys中,这一次就不用再输入
john@john MINGW64/$ rm -rf project/
john@john MINGW64/$ git clone git@192.168.159.128:project
Cloning into 'project'...
warning: You appear to have cloned an empty repository.
```

## 拒绝ssh登陆git服务器

```shell
#退回到ubuntu原来的用户
ls@ls:~$ cat /etc/shells   # see if `git-shell` is already in there.  If not...
ls@ls:~$ which git-shell   # make sure git-shell is installed on your system.
ls@ls:~$ sudo vim /etc/shells  # and add the path to git-shell from last command
#使用 chsh <username> 命令修改任一系统用户的 shell
ls@ls:~$ sudo chsh git
正在更改 git 的 shell
请输入新值，或直接敲回车键以使用默认值
        登录 Shell [/bin/bash]: /usr/bin/git-shell
#在本地测试是否能通过ssh协议登陆git服务器
john@john MINGW64/$ ssh git@gitserver
fatal: Interactive git shell is not enabled.
hint: ~/git-shell-commands should exist and have read and execute access.
Connection to gitserver closed.
#这样，用户 git 就只能利用 SSH 连接对 Git 仓库进行推送和拉取操作，而不能登录机器并取得普通 shell。
```

参考:[服务器上的 Git - 配置服务器](https://git-scm.com/book/zh/v2/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E9%85%8D%E7%BD%AE%E6%9C%8D%E5%8A%A1%E5%99%A8)