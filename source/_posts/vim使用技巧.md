---
title: vim使用技巧
date: 2018-05-10 19:52:57
tags: [vim, linux, 工作效率]
categories: vim
---

书中代码所在路径：[Source Code for **Practical Vim**](https://pragprog.com/titles/dnvim/source_code)

1. vim查找字符串

   ```shell
   #Normal模式下找“main”这个单词
   /main #按n能找到所有包含main的词
   #精确匹配main这个词
   /\<main\>
   #反向查找
   ?test #按n可反向查找test字符串
   #利用正则匹配查找字符串p1,p2
   /p\d
   ```

2. vim查找替换

   ```shell
   :%s/foo/bar/g #在普通模式中输入,这个命令会将文内所有foo替换为bar
   :%s/^/要插入的字符串 #每行的行首都添加一个字符串
   :%s/$/要插入的字符串 #每行的行尾都添加一个字符串
   ```

3. 跳转

   - 标记

   ```shell
   #m{letter} 可以在当前光标位置创建一个位置标记，然后用`{letter}就可以使光标快速回到标记所在之处。
   #letter为大写字母时，为全局标记，可用于文件间的跳转；
   #letter为小写字母时，为局部缓冲区的标记，可用于当前文件的跳转。
   #Vim会自动创建一些位置标记
   "``"回到上次光标所在的位置
   "`."标记总是指向上次修改的位置
   "`^"标记则会记录上次退出插入模式时光标所在的位置。
   ```

   - 文件跳转与回退前进

   ```shell
   #gf命令可跳转到光标下的文件。有可能需要制定文件的扩展名(:set suffixesadd+=.rb)
   #<C-o>命令可返回原处，相当于回退；<C-i>命令与之相反
   ```

   - 多文件跳转

   ```tex
   *********************
   1.txt
   *********************
   dfklsfjsfs
   
   *********************
   2.txt
   *********************
   dfklsfjsfs
   var_a = 99;
   var_b =9;
   sfjdsfl
   skfdlsfkjdsf
   
   *********************
   3.txt
   *********************
   dfklsfjsfs
   
   *********************
   2.txt
   *********************
   dfklsfjsfs
   var_a = 99;
   var_b =9;
   
   *********************
   2.txt
   *********************
   PI
   ```

   使用quickfix在多文件中进行工作

   ```shell
   $ vim 1.txt 2.txt 3.txt 4.txt #查找var_a
   ```

   输入 `:vim /var_a/##`，此时若想以单独列表窗口形式查看查找结果，可以用`:copen`来打开列表，显示列表为

   ```shell
   2.txt|2col 1| var_a = 99;
   4.txt|2col 1| var_a = 99;
   ```

   在quickfix窗口中，按h, j, l, k可以正常跳转，`:cc 5`: 跳过编号为 5 的；`:cn`：跳至下一编号；`cp`：跳至上一编号；`:cclose`：关闭 quickfix 。如果想要在特殊后缀的文件中查找某个字符串，可以输入`:vim /PI/*.c`

   参见：[VIM 超实用使用经验](https://gitbook.cn/gitchat/activity/5b600045baffd578157e4d88)

4. vim设置tab为4个空格

```shell
:set ts=4
:set expandtab
:%retab!
```

参考：[vim tab设置为4个空格](https://blog.csdn.net/jiang1013nan/article/details/6298727)

5. vimdiff比较两个文件的差异

> 差异点跳转(http://www.cnblogs.com/abeen/p/4255754.html)
>
> - ]c 下一个差异点
> - [c 上一个差异点
> - 按`dp`复制当前文件的内容到另一个文件
> - 按`do`复制另一个文件的内容到当前文件
> - 在两个文件间跳转:`ctrl-w-w`

6. 设置自动折行(https://blog.csdn.net/songbohr/article/details/5098855)

   ```shell
   #是把长的一行用多行显示 , 不在文件里加换行符用
   :set wrap 设置自动折行
   :set nowrap 设置不自动折行
   ```

7. 中文乱码解决办法

   在`~/.vimrc`文件中添加`set fileencodings=utf-8`






















