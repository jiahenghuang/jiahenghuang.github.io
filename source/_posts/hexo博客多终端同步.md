---
title: hexo博客多终端同步
mathjax: false
date: 2018-09-28 09:17:33
tags:
- hexo
- 同步
categories: hexo
---

# 1.静态页面的同步

修改站点配置文件`_config.yml`如下，注意pi和github两个仓库的master前均不能有空格，否则会报错`fatal: remote part of refspec is not a valid name in HEAD: master`

```shell
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo:
      pi: ssh://git@example.com:8083/home/git/blog/hexo.git,master
      github: git@github.com:LjessonS/LjessonS.github.io.git,master
```

然后按照原来的部署方式部署就行了`hexo clean && hexo d -g`。

# 2.博客源文件的同步

- 第一种方法

```shell
#直接修改git配置文件
$ vim .git/config
[remote "web"]
url = ssh://git@example1.com:8083/home/git/blog/hexo.git
url = ssh://git@example2.com:8083/home/git/blog/hexo.git
```

- 第二种方法

```shell
#通过命令行的方式,本质上也是修改的.git/config配置文件
$ git remote add web ssh://git@example1.com:8083/home/git/blog/hexo.git
$ git remote set-url --add web ssh://git@example2.com:8083/home/git/blog/hexo.git
$ git push all --all
```

参见：

- [将hexo博客同时托管在github，oschina和coding](https://www.jianshu.com/p/18ffe8148ef7)