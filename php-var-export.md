---
title: php  var_export
date: 2018-04-28 21:25:13
tags:
---
var_export() 函数返回关于传递给该函数的变量的结构信息，

它和 var_dump() 类似，

不同的是其返回的表示是合法的 PHP代码
var_export必须返回合法的php代码， 也就是说，
var_export返回的代码，可以直接当作php代码赋值个一个变量。 
而这个变量就会取得和被var_export一样的类型的值。
    
   for example:
```
    $var=array('1'=>'A','2'=>'B','3'=>'C','4'=>'D');
    var_export($var);
    var_dump($var);
```
  

   返回结果为：
```
    array ( 1 => 'A', 2 => 'B', 3 => 'C', 4 => 'D', )
```
```
 array (size=4)
  1 =>  'A' (length=1)
  2 =>  'B' (length=1)
  3 =>  'C' (length=1)
  4 =>  'D' (length=1)
```

```
  $var =var_export(array('a','b',array('aa','bb','cc')),TRUE)，加上TRUE后，不会再打印出来，而是给了一个变量，这样就可以直接输出; 
  
  $arr=(array("a"=>1,"b"=>2));
  writelog("sqllog",var_export($arr,true));
echo $var;

```