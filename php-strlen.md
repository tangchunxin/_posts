---
title: PHP-strlen
date: 2018-04-17 22:52:55
tags:
---

## strlen 与 mb_strlen区别  面试经常被问到

#### 在PHP里有两个计算字符串个数的函数
一个是 strlen,一个是mb_strlen;
先来看看手册中的定义
- strlen
- strlen — 获取字符串长度
- int strlen ( string $string )
返回给定的字符串 string 的长度。
- mb_strlen
- int mb_strlen ( string $str [, string $encoding ] )
- 返回给定的字符串 string 的长度。

encoding参数为字符编码。如果省略，则使用内部字符编码。
这么看除了mb_strlen可以传递一个字符编码好像没有其他区别，下面通过例子，讲解这两者之间的区别。
先看例子：
```
<?php    //测试时文件的编码方式要是UTF8    
$str='中文a字1符';    
echo strlen($str).'<br>';//14    字节 中文*3
echo mb_strlen($str,'utf8').'<br>';//6 字符个数,中文也算1
echo mb_strlen($str,'gbk').'<br>';//8  一个中文1.5个字符
echo mb_strlen($str,'gb2312').'<br>';//10 一个中文2个字符??待却定
```

结果分析：在strlen计算时，对待一个UTF8的中文字符是3个长度，所以“中文a字1符”长度是3*4+2=14,在mb_strlen计算时，选定内码为UTF8，则会将一个中文字符当作长度1来计算，所以“中文a字1符”长度是6 

----
ps:
#### 采用mb_strlen函数可以较好地解决这个问题。mb_strlen的用法和 strlen类似，只不过它有第二个可选参数用于指定字符编码。例如得到UTF-8的字符串$str长度，可以用 mb_strlen($str,'UTF-8')。如果省略第二个参数，则会使用PHP的内部编码。内部编码可以通过 mb_internal_encoding()函数得到。
#### 需要注意的是，mb_strlen并不是PHP核心函数，Windows 下使用前需要确保在php.ini中加载了php_mbstring.dll，即确保“extension=php_mbstring.dll”这一行存在并且没有被注释掉，否则会出现未定义函数的问题。Linux 下需要编译这个扩展。
