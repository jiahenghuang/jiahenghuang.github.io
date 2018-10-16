---
title: mysql笔记
comments: true
mathjax: false
date: 2018-10-16 17:31:41
tags:
- mysql
- notes
categories: mysql
---

# 1.创建数据库

```shell
#登陆
$ mysql -u root -p
Enter password:******  # 登录后进入终端

mysql> create DATABASE databasename;
```

# 2.从远程mysql数据库中导出数据

```shell
mysqldump -hhostname -uusername -ppassword databasename #hostname为远程主机名，password为密码，为了安全最好不要直接指定-p参数，databasename为所要导出的数据库名
#如果需要导出的数据库非常大，可以用gzip压缩
mysqldump -hhostname -uusername -ppassword databasename | gzip > backupfile.sql.gz

#还原压缩的mysql数据库，并导入到mysql数据库，注意要提前在新的mysql数据库建好名为databasename的数据库
gunzip < backupfile.sql.gz | mysql -uusername -ppassword databasename
```

参见：[mysqldump导出导入远程数据库](https://blog.csdn.net/dodo0_0/article/details/4921812)