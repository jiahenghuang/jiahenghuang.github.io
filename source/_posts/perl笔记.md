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

# 2.perl调试

```shell
$ perl -d test.pl
```

调试命令：

- n: 执行下一步
- b  linenum: 在某一行添加断点
- B linenum: 删除某行的断点
- l: 列出执行到所在行附近的代码
- L: 查看设置的断点

## 2.1让perl调试能够使用退格键+方向键

从CPAN上下载并安装perl模块`Term::ReadLine::Gnu`即可

参考：[Perl debug and backspace](https://blog.csdn.net/Braveo/article/details/2849901)

# 3.获取当前脚本的绝对路径

```perl
#!/usr/bin/perl -w 
use File::Basename;
use Cwd 'abs_path';
use strict;

my $bin = undef;
$bin = dirname(abs_path($0));
print $bin."\n";
```

