---
title: perl笔记
date: 2018-09-11 16:01:23
tags:
- perl
- notes
mathjax: false
categories: perl
---

# 1.列表的引用与嵌套数据结构

```shell
#Perl的语法非常宽容，它允许你写出有歧义的或者有不可预期行为的代码。我不会去解释这些诡异的行为是什么样的，因为你最好避开它们。避免这种情况的方法是在你写的每个Perl脚本或者模块的开头加上use strict; use warnings;。use foo;这种语句叫做编译指示（pragmas），编译指示是给perl.exe的一个提示，在程序开始执行之前的语法验证阶段会发挥作用，脚本语句实际执行的时候这些编译指示对于运行结果没有影响。
use strict;
use warnings;

my %account = (
	"number" => "31415926",
	"opened" => "3000-01-01",
	"owners" => [
		{
			"name" => "Philip Fry",
			"DOB"  => "1974-08-06",
		},
		{
			"name" => "Hubert Farnsworth",
			"DOB"  => "2841-04-09",
		},
	],
);

print "Account #", $account{"number"}, "\n";
print "Opened on ", $account{"opened"}, "\n";
print "Joint owners:\n";
print "\t", $account{"owners"}->[0]->{"name"}, " (born ", $account{"owners"}->[0]->{"DOB"}, ")\n";
print "\t", $account{"owners"}->[1]->{"name"}, " (born ", $account{"owners"}->[1]->{"DOB"}, ")\n";
```

参见[两个半小时学会Perl](https://qntm.org/files/perl/perl_cn.html)