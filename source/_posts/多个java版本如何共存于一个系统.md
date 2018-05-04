---
title: 多个java版本如何共存于一个系统
date: 2018-05-04 17:56:07
tags: java, 环境变量
categories: java
---

# 环境变量设置			

```shell
JAVA6_HOME = { Location of Java 6 Installation }
JAVA7_HOME = { Location of Java 7 Installation }
JAVA_HOME = %JAVA7_HOME% - this variable will change, when you want to a different JDK.
```

> adding %JAVAHOME%\bin to your PATH make sure that it appears before C:\Windows\System32

参考：https://coderwall.com/p/gbek2g/java-6-and-java-7-on-windows