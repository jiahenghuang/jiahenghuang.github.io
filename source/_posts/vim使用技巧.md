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
   ```

2. vim查找替换,这个命令会将文内所有foo替换为bar

   ```shell
   :%s/foo/bar/g #在普通模式中输入
   ```

3. 跳转

   ```latex
   m{letter} 可以在当前光标位置创建一个位置标记，然后用`{letter}就可以使光标快速回到标记所在之处。
   letter为大写字母时，为全局标记，可用于文件间的跳转；
   letter为小写字母时，为局部缓冲区的标记，可用于当前文件的跳转。
   ```

   ```latex
   gf命令可跳转到光标下的文件。有可能需要制定文件的扩展名(:set suffixesadd+=.rb)
   <C-o>命令可返回原处，相当于回退；<C-i>命令与之相反
   ```

   ```latex
   Vim会自动创建一些位置标记
   "`."标记总是指向上次修改的位置
   "`^"标记则会记录上次退出插入模式时光标所在的位置。
   ```

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

























