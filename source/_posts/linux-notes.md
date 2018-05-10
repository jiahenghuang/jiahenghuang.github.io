---
title: linux notes
date: 2018-04-30 15:38:18
tags: 
- linux
- notes
categories: 
- linux
---

# linux使用技巧

记录一些有用有趣的技巧。

<!-- more -->

## 1.添加别名（对git bash也适用）

```shell
$ alias #可通过该命令查看系统中变量别名
$ vim ~/.bashrc #为常用工作路径添加别名，方便直接进入
alias gss='cd /d/share/work/codeSpace/git/LjessonS.github.io/source/_posts'
alias ...='cd ..' #输入...直接能进入上层目录
$ source ~/.bashrc #让别名生效
```

## 2.创建博客自动用编辑器打开markdown文件

在站点根目录scripts文件夹，若没有则创建一个，然后在scripts文件夹中创建一个auto_open.js的脚本文件

```shell
$ mkdir scripts 
$ vim scripts/auto_open.js
```

输入如下脚本：

```javascript
var spawn = require('child_process').exec;
// Hexo 2.x 用户复制这段
//hexo.on('new', function(path){
//  spawn('start  "markdown编辑器绝对路径.exe" ' + path);
//});
//C:\Program Files\Typora\Typora.exe 是typora编辑器在我本地的路径！
// Hexo 3 用户复制这段
hexo.on('new', function(data){
  spawn('start  "C:\Program Files\Typora\Typora.exe" ' + data.path);
});
```

这样，以后每次新建一个博客都能用typora直接打开。效果图如下：

![微信图片_20180430162139](linux-notes/微信图片_20180430162139.png)

但是如果我们要编辑以前写的笔记该怎么打开呢。方法是将typora添加到windows系统变量中

```shell
$ cd source/_posts
$ Typora.exe linux-notes.md #事先已经将typora添加进系统环境变量
```

效果如下

![微信截图_20180430162618](linux-notes/微信截图_20180430162618.png)

参考链接：

1. [Hexo 建立新文章後自動打開編輯器跟瀏覽器](https://thisis577.github.io/2016/08/05/auto-open-editor-and-browser-after-hexo-new-post/)
2. [hexo 写文章创建文件自动打开编辑器！](https://www.jianshu.com/p/5fbb62791f9b)

## 3.命令的使用

- mv命令的使用，移动多个文件到目标文件夹 mv -t DESTINATION file1 file2 file3
- rm命令的使用，删除文件夹 rm -rf folderName, incase  doesn't have permission:sudo rm -rf folderName.
- unzip命令解压缩文件（参考：http://blog.csdn.net/shenyunsese/article/details/17556089）

```shell
$ unzip -o -d /home/sunny myfile.zip #把myfile.zip文件解压到 /home/sunny/
-o:不提示的情况下覆盖文件；
-d:-d /home/sunny 指明将文件解压缩到/home/sunny目录下
$ unzip myfile.zip #把myfile解压到当前目录，不删除myfile.zip文件
```
- gzip

  ```shell
  #解压缩：
  $ gzip -dk xxx.fq.gz #d参数表示解压缩，k参数表示保留输入文件fq.gz压缩包，默认不保留输入文件。
  #压缩：
  $ gzip -fk xxx.fq #若先前有压缩过但没有压缩完成得到xxx.fq.gz文件，用f参数可以覆盖重新打包xxx.fq文件为xxx.fq.gz
  #指定k参数是保留输入文件xxx.fq的意思，默认不保留输入文件。
  ```

- nohup与&命令(http://blog.csdn.net/liuyanfeier/article/details/62422742)：

  - &：当在前台运行某个作业时，终端被该作业占据；可以在命令后面加上& 实现后台运行。

  - nohup:  使用&命令后，作业被提交到后台运行，当前控制台没有被占用，但是一但把当前控制台关掉(退出帐户时)，作业就会停止运行。nohup命令可以在你退出帐户之后继续运行相应的进程。nohup就是不挂起的意思( no hang up)。

    ```shell
    #该命令的一般形式为
    nohup command &
    #如果使用nohup命令提交作业，那么在缺省情况下该作业的所有输出都被重定向到一个名为nohup.out的文件中，除非另外指定了输出文件
    $ nohup command > myout.file 2>&1 &
    $ nohup sh exc.sh 1>exc.e 2>exc.o & #经常是标准错误重定向到exc.e,标准输出重定向到exc.o
    ```

- sort命令对文本内容按列排序（默认按升序排序）：参考https://segmentfault.com/a/1190000005713784

  ```shell
  #对文件按第9列（-k 9）进行数值（加上n）排序，最后用less
  $ sort -k 9n hdcancer_soap_align.txt | less
  #对文件首先按第8列（-k 8）进行字符（默认）排序	
  $ sort -k 8 -k 9n hdcancer_soap_align.txt | less
  #对文本先排序后去重, c参数表示带上重复次数：
  $ cat refGene.gff | awk '{print $1}' | sort | uniq -c 
  ```

- 杀掉进程

  ```shell
  $ kill -9 pid_number
  ##杀掉python产生的所有进程：
  for i in `ps -ef |grep python3|cut -c 7-15`; do sudo kill -9 $i;done
  for i in `ps -ef |grep python|cut -c 7-15`; do sudo kill -9 $i;done
  ```

- linux系统获取管理员权限:`sudo su`然后再输入密码。

- 查看系统已用存储和剩余存储的命令:df -h .；du -sh file

- linux文件权限表示及查看修改方法

  > 所有者(user)、群组(group)、其他人(other):
  > 	-rw-rw-r--
  > 	第一个rw- 代表的是所有者（user）拥有的权限
  > 	第二个rw 代表的是组群（group）拥有的权限
  > 	最后那个r--代表的是其他人（other）拥有的权限
  > 文件权限表示：
  > 	r 表示文件可以被读（read）
  > 	w 表示文件可以被写（write）
  > 	x 表示文件可以被执行（如果它是程序的话）
  > 	- 表示相应的权限还没有被授予
  > 文件权限查看：
  > 	如果有文件夹  /a/b/c
  > 	那么执行 ls -l /a/b 查看权限的文件并不是b，而是查看的c的权限。
  > 	ls -l /a 查看的是b文件的权限
  > 	ls -l /a/b 查看的是c文件的权限
  > 	ls -l /a/b/c 查看的是c文件的权限
  > 文件权限修改方法：
  > 	使文件可以直接执行的命令：chmod +x filename
  > 	使所有用户对目录都有读写权限：sudo chmod ugo+rw /opt
  > ln命令：
  > 	ln是linux中又一个非常重要命令，它的功能是为某一个文件在另外一个位置建立一个
  > 	同步的链接.当我们需要在不同的目录，用到相同的文件时，我们不需要在每一个需要
  > 	的目录下都放一个必须相同的文件，我们只要在某个固定的目录，放上该文件，然后
  > 	在 其它的目录下用ln命令链接（link）它就可以，不必重复的占用磁盘空间。

- tail -n 1000 filename：显示文件最后1000行

  ```shell
  $ tail -n +1000 filename：从1000行开始显示，显示1000行以后的
  $ head -n 1000 filename：显示前面1000行
  #打印第三列含有“chrX”的行文本：
  $ cat 15B0048464.sort.dup.sam | awk '{if($3=="chrX"){print}}' | less
  ```

- echo "Still_waters_run_deep" 1>&2（参考http://www.jb51.net/article/64183.htm）：

  > & 是一个描述符，如果1或2前不加&，会被当成一个普通文件。
  >
  > 1>&2 意思是把标准输出重定向到标准错误.
  > 2>&1 意思是把标准错误输出重定向到标准输出。
  > &>filename 意思是把标准输出和标准错误输出都重定向到文件filename中

- 统计文件行数

  ```shell
  wc [option] file
  -c统计字节数、-l统计行数、-w统计字数
  #例如：
  $ wc -lcw file1 file2
  4 33 file1
  7 52 file2
  11 11 85 total
  ##输出所有字符"+"，再用wc管道操作得出+出现的次数即可得到样本文件的read数
  grep -o '+' clean_15B0048464_1.fq clean_15B0048464_2.fq | wc -l	
  ```

- 将远程文件目录（指定-r参数，不指定-r是对文件进行操作）拷贝到本地

  ```shell
  $ scp -r username@172.16.56.201:/home/ls/15B0048464_raw_1.fq.gz ./
  ```

- 文件合并（http://www.cnblogs.com/giraffe/p/3193085.html）

  ```shell
  #第二：两个文件合并
  #一个文件在上，一个文件在下
  $ cat file1 file2 > file3
  #一个文件在左，一个文件在右
  $ paste file1 file2 > file3
  ```

- grep

  ```shell
  grep ‘asb’ xxx.sam -A 5 #可以显示含有‘asb’所在行的后五行
  #输出文本的非注释行：cat gatk.variation.vcf | grep -v "^#" | wc -l
  ```

- ln

  ```shell
  #注：虚拟机ubuntu与windows共享磁盘，软链接与硬链接都不可行。
  $ ln source.sh destination.sh #默认硬链接
  $ ln -s source.sh destination.sh #软链接
  ```

  - 硬链接与软链接区别
    1. 硬链接实际是源文件建立一个别名，本质上是同一个文件；而软链接实际是创建了一个源文件的索引文件，和源文件是两个文件
    2. 硬链接：删除源文件或目标文件，对另外一个链接源文件的文件不影响，即源文件内容不会删除，但是修改源文件或目标文件，所有其他链接源文件的文件内容也会随之改变。
    3. 软链接：删除源文件，则软链接不可用，删除软链接文件，则不影响源文件。修改目标文件会影响源文件和硬链接一样，修改源文件也会影响目标文件。
    4. 对硬链接和软链接目标文件所造成的修改，其他被源文件软链接和硬链接的文件也会被造成同样的修改。

- scp

  ```shell
  #scp一次从远程传输多个文件（http://www.cnblogs.com/voidy/p/4215891.html）：
  scp xxx@172.16.33.76:/home/xxx/test/\{CL100036475_L01_85.sort.dup.bam,CL100036475_L01_85.sort.dup.bam.bai\} .
  ```

- uniq: -d参数只显示重复行。http://os.51cto.com/art/200912/171332.htm

- for循环查找重复行，了解SortSam排序机制：按染色体号和位置进行排序（sam文件的第3,4列），然后这两个列若相同则按源文件中顺序排序

  ```shell
  for i in (cat CL100036475_L01_85.sort.sam | awk '{print $4}'| uniq -d | head -33); do grep "i" CL100036475_L01_85.sort.sam; done
  ```
  ​


## 4.ubuntu使用中的一些问题

### 4.1 解决语言设置错误的问题

参考：（http://wenzhixin.net.cn/2014/01/11/ubuntu_setting_locale_failed）

> perl: warning: Falling back to the standard locale ("C").
> 	安装 localepurge 管理语言文件
> 	sudo apt-get install localepurge
> 	选择我们想要的语言，例如 en_US.UTF-8 和 zh_CN.UTF-8。
> 	当然也可以使用以下命令再次进行配置：
> 	sudo dpkg-reconfigure localepurge
> 	生成自己想要的语言
> 	sudo locale-gen zh_CN.UTF-8 en_US.UTF-8
> 	打印出当前的配置信息
> 	locale
> 	到此，搞定！！！
> 	默认情况下终端 ssh 的时候会将本地的 locale 传到服务器中，可以通过命令指定 ssh 服务器的语言：LC_ALL=en_US.UTF-8 ssh bgi902@172.16.56.201

解决ubuntu bash中文乱码的问题:
	http://nekomiao.me/2016/08/23/ubuntu-zh-CN/	

查看linux是32位还是64位的（[五种方法检测你的 Linux 是32位还是64位](https://linux.cn/article-3048-1.html)）

```shell
$ uname -a #x86_64说明是64位
#Linux gpudev_node1 4.4.0-121-generic #145-Ubuntu SMP Fri Apr 13 13:47:23 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
$ uname -m #直接输出
#x86_64
```

### 4.2 notepadqq关闭异常

参考:https://forum.manjaro.org/t/notepadqq-doesnt-save-preference/23867

```shell
#Error while trying to save this session. Please ensure the following directory is accessible:/home/user/.config/Notepadqq/tabCache
#解决办法：
$ sudo chown -R ls:ls $HOME/.config/Notepadqq
$ chmod -R ug+rwx $HOME/.config/Notepadqq	
```

### 4.3 用UltraISO制作Ubuntu16.04 U盘启动盘

参考：http://blog.csdn.net/yaoyut/article/details/78003061



## 5.本地winscp+secureCRT连接虚拟机linux系统

1. winscp直接下载不需要激活

2. secureCRT（64位）需要激活（下载软件自带注册机）

3. 连接本地虚拟机linux系统（http://www.linuxidc.com/Linux/2016-12/138786.htm、http://www.cnblogs.com/xuliangxing/p/4462929.html）

   - windows ping通linux：ping 192.168.116.143

     ubuntu ping通windows: ping 172.16.56.8

   - 安装SSH（出现了ssh: connect to host localhost port 22: Connection refused 说明你的机器没装SSH）：

     sudo apt-get install openssh-server

   - 查看是否有进程在22端口上监听

     netstat -nat | grep 22

   - 关闭掉防火墙

     sudo ufw disable

   - 接着就可以使用winscp和secureCRT连接虚拟机了

   - SecureCRT连接Linux终端中文乱码解决方法(http://blog.csdn.net/zl1zl2zl3/article/details/52237940）查看CRT的session option，字符编码修正为UTF-8。

   - 永久修改背景色

			告诉你从哪里永久设置背景色：http://blog.sina.com.cn/s/blog_a0db295e0100wh2x.html
		设置的颜色怎么舒服点：http://www.voidcn.com/article/p-rfuzpsmu-up.html


## 6. clion安装与卸载

参考：http://www.voidcn.com/article/p-evedqfrg-kd.html

```shell
$ tar -zxvf CLion-2016.3.5.tar.gz#解压
#进入bin目录,执行sh文件：
$ ./clion.sh
$ sudo notepadqq /etc/hosts##添加0.0.0.0 account.jetbrains.com进hosts文件
#添加激活码：http://idea.lanyus.com/
```

**卸载**

> Delete the installation directory
> Delete the "config" and "system" configuration directories. These contain IntelliJ IDEA's caches, configuration and plugins.`（~/.<PRODUCT><VERSION>）`

