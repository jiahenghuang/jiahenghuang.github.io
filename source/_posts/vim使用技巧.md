---
title: vim使用技巧
date: 2018-05-10 19:52:57
tags: [vim, linux, 工作效率]
categories: vim
---

1. vim查找字符串

   ```shell
   #Normal模式下找“main”这个单词
   /main #按n能找到所有包含main的词
   #精确匹配main这个词
   /\<main\>
   ```

   ​