---
title: linux-notes
date: 2018-04-30 15:38:18
tags: linux, notes
category: linux
---

# linux使用技巧

## 1.添加别名（对git bash也适用）

```shell
$ alias #可通过该命令查看系统中变量别名
$ vim ~/.bashrc #为常用工作路径添加别名，方便直接进入
alias gss='cd /d/share/work/codeSpace/git/LjessonS.github.io/'
alias ...='cd ..' #输入...直接能进入上层目录
$ source ~/.bashrc #让别名生效
```

