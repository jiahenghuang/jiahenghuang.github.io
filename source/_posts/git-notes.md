---
title: git notes
date: 2018-05-10 19:28:07
tags: git
categories: git
---

git的常用命令

# 1.分支

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

6. 删除分支

   ```shell
   #删除本地分支
   git branch -D branchName 
   #删除远程分支
   git push origin :branchName
   ```

7. 为分支打上tag

   ```shell
   $ git tag -m "Tag version 1.0" V1.0 3ede462 #V1.0是当前提交3ede462的标签
   ```


# 2.提交代码

1. 多人协作提交代码时如何方便的解决冲突

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

2. git push免密登陆

   ```shell
   #如果已经使用https协议克隆了，那么按照如下方法更改协议： 
   git remote set-url origin git@github.com:yourusername/yourrepositoryname.git
   ```

   参考：[git push 免密码](https://bryceyang.github.io/blog/2017/05/25/Tips/#heading-git-push-%E5%85%8D%E5%AF%86%E7%A0%[81)、[杨凯的回答](https://www.zhihu.com/question/31836445)

3. 提交时使用vim作为默认编辑器

   ```shell
   $ export GIT_EDITOR=vim #在.bashrc中添加
   $ source .bashrc
   ```

4. 暂存工作区（https://blog.csdn.net/daguanjia11/article/details/73810577）

   ```shell
   $ git stash #保存当前工作进度，会把暂存区和工作区的改动保存起来。
   $ git stash pop #恢复最新的进度到工作区。git默认会把工作区和暂存区的改动都恢复到工作区。
   ```

5. git push遇到push.default警告的问题

   ```shell
   git config --global push.default matching #当 push.default 的值设置成 ‘matching’ ，git 将会推送所有本地已存在的同名分支到远程仓库
   git config --global push.default simple #从 Git 2.0 开始，git 采用更加保守的值'simple'，只会推送当前分支到相应的远程仓库，'git pull' 也将只更新当前
   ```

   最好是用采用第二种方法，只推送当前分支。

   参见：[Git 2.x 中git push时遇到 push.default 警告的解决方法](https://www.jianshu.com/p/e26175b2e916)