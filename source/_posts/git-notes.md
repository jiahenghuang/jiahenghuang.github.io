---
title: git notes
date: 2018-05-10 19:28:07
tags: git
categories: git
---

git的常用命令

<!--more-->

1. git创建分支

   ```shell
   git branch local
   ```

2. 切换分支

   ```shell
   git checkout local
   ```

3. 比较master分支与当前本地分支某文件的差异

   ```shell
   git diff master _posts/analysis.md _posts/analysis.md
   ```

4. 将本地local分支commit的更新修改到主分支

   ```shell
   git merge local
   ```

5. git修改分支名

   ```shell
   git branch -m master hexo
   ```

6. 多人协作提交代码时如何方便的解决冲突

   ```shell
   #先将代码备份，再执行下行命令
   rm main.cpp #删掉自己写的代码
   git checkout --theirs main.cpp
   git pull #这样本地就是服务器中的存在的最新代码
   #然后将pull下来的代码用beyondCompare软件与备份的代码比较，把自己的修改添加进去
   git add main.cpp 
   git commit -m "message"
   git pull #标准操作，在push之前尽量pull一下
   git push #提交自己的更新
   ```

   ​