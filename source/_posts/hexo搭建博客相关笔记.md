---
title: "hexo搭建博客相关笔记"
date: 2018-08-26 19:58:15
tags: [hexo, notes，博客]
categories: hexo
---

# 1. node.js相关笔记

- 安装cnpm：参见[npm不是内部或外部命令 cnpm: command not found 解决方案](https://blog.csdn.net/xuelang532777032/article/details/64904971)

  - `npm install cnpm -g --registry=https://registry.npm.taobao.org `
- cnpm与npm的关系:参见[cnpm与npm的区别](https://blog.csdn.net/chi1130/article/details/72773278)
  1. cnpm与npm命令是一样的，只不过cnpm走的是国内淘宝镜像，要快一些
  2. npm带上`-g` 是全局安装

- npm install 报错出现Unexpected end of JSON input while parsing near

  1. `npm config set registry https://registry.npm.taobao.org`
  2. `#清除缓存：npm cache clean --force`
  3. `npm install`安装成功后，即可使用`hexo clean`命令了

  参见https://juejin.im/post/5a804a926fb9a0634c266c31