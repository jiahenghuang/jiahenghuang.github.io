---
title: 在树莓派上基于git搭建hexo博客
date: 2018-09-02 10:30:48
tags:
- 树莓派
- hexo
- git
categories:
- 树莓派
- hexo
---

以下是技术路线

- 在树莓派上创建git用户
- 创建git仓库
- 安装并配置nginx
- 本地配置hexo

<!--more-->

# 1. 在树莓派上创建git用户

参见前面的文章[在linux上搭建git服务器](https://ljessons.github.io/2018/08/23/%E5%9C%A8linux%E4%B8%8A%E6%90%AD%E5%BB%BAgit%E6%9C%8D%E5%8A%A1%E5%99%A8/)

**注：**先不要着急设置拒绝ssh访问，后面还要在服务器上通过git用户进行一些操作的

# 2. 创建git仓库

```shell
git@raspberrypi:~ $ mkdir blog
git@raspberrypi:~ $ cd blog
git@raspberrypi:~/blog $ mkdir hexo #网站目录
git@raspberrypi:~/blog $ git init --bare hexo.git #博客仓库

#配置git hooks-------------------------
git@raspberrypi:~/blog $ sudo vim /home/git/blog/hexo.git/hooks/post-receive
#编辑修改post-receive文件，输入以下内容保存
#!/bin/bash
GIT_REPO=/home/git/blog/hexo.git #git仓库，需要修改
TMP_GIT_CLONE=/tmp/hexo
PUBLIC_WWW=/home/git/blog/hexo #网站目录，需要修改
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}

#赋予脚本的执行权限,很重要！！！不然网站目录/home/git/blog/hexo没有文件，后面部署hexo打开你的网站时就会出现403forbidden
git@raspberrypi:~/blog $ chmod +x /home/git/blog/hexo.git/hooks/post-receive
```

# 3. 安装并配置nginx

```shell
git@raspberrypi:~/blog $ sudo apt-get install nginx

git@raspberrypi:~/blog $ vim /etc/nginx/sites-available/default
#找到root, server_name修改成你自己的,其他的内容可以不用修改，我用...表示了
...
server{
...
	listen 80 default_server;
    listen [::]:80 default_server;
    root /home/git/blog/hexo;
    server_name 192.168.0.103;
...
}
...

#重启Nginx
git@raspberrypi:~/blog $ sudo service nginx restart
```

# 4. 本地配置hexo

这里hexo的安装就不说了

```shell
ls@DESKTOP-OV3HILK MINGW64 /d/hexo $ hexo init #注意这个目录下面要是空的，才可以运行这个命令
ls@DESKTOP-OV3HILK MINGW64 /d/hexo $ vim _config.yml #修改站点配置文件

#照着修改下面的url和deployment，先看看简单的hello-world能不能成功，后面再进一步优化设置主题
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://192.168.0.103 #需要修改
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@192.168.0.103:/home/git/blog/hexo.git 
  branch: master
```

关联本地博客与树莓派上的hexo仓库，将博客文件放在hexo分支，主分支用来存放静态页面文件

```shell
ls@DESKTOP-OV3HILK MINGW64 /d/hexo $ git init
ls@DESKTOP-OV3HILK MINGW64 /d/hexo (master)$ git init

#提交更新，便于修改分支
ls@DESKTOP-OV3HILK MINGW64 /d/hexo (master)$ git add .
ls@DESKTOP-OV3HILK MINGW64 /d/hexo (master)$ git commit -m "test"
#将master分支名修改为hexo分支,hexo分支用来存放博客的源文件
ls@DESKTOP-OV3HILK MINGW64 /d/hexo (master)$ git branch -m master hexo
ls@DESKTOP-OV3HILK MINGW64 /d/hexo (hexo)$ git remote add origin git@192.168.0.103:/home/git/blog/hexo.git
#推送博客源文件到远程hexo分支
ls@DESKTOP-OV3HILK MINGW64 /d/hexo (hexo)$ git push origin hexo
#部署博客到树莓派git服务器
ls@DESKTOP-OV3HILK MINGW64 /d/hexo (hexo)$ hexo clean && hexo d -g
```

完成上面的步骤后，打开就可以看到hexo的 hello world了，主题有点不正常，不过问题不大，后面继续优化就是了

![](在树莓派上基于git搭建hexo博客/20180902113046.png)

参考:[在VPS上搭建hexo博客，利用git更新](https://tiktoking.github.io/2016/01/26/hexo/)