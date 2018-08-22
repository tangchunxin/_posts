---
title: php 导出 excel 简单方式
date: 2018-05-09 22:02:08
tags:
---

php 导出数据到excel方式  

#### php 导出 txt  简单方式


 ```
   //下载文件名
    $file_name = $name.date('Y.m.d.H-i-s',time());
    $filename = iconv('UTF-8', 'GBK', $file_name);
    header('Content-Type: text/plain');
    header ( 'Content-Disposition: attachment;filename="' . $filename . '.txt"' ); 
    header ( 'Cache-Control: max-age=0' );
    echo "我是.txt的内容";
```

#### php 导出 excel  简单方式

```
<?php
$pname = "ces";
$url = "aaa";
$icp = "icl";
$str = "平台ID\t平台名称\t首页网址\t\n";
$str = iconv('utf-8','gbk',$str);
$str .= trim($pname)."\t".trim($url)."\t".trim($icp)."\t\n";

 $filename = '平台档案导出'.date('Ymd').'.xls';

 header("Cache-Control: must-revalidate, post-check=0, pre-check=0");
 header("Content-Type: application/vnd.ms-execl");
 header("Content-Type: application/force-download");
 header("Content-Type: application/download");
 header("Content-Disposition: attachment; filename=".$filename);
 header("Content-Transfer-Encoding: binary");
 header("Pragma: no-cache");
 header("Expires: 0");

 echo $str;
 
 ```
 
 header部分可修改为
 
 ```
 header('Content-Description: File Transfer');
header('Content-Type: application/vnd.ms-excel');
header('Content-Disposition: attachment; filename="'. $filename .'"');
header('Expires: 0');
header('Cache-Control: must-revalidate');
header('Pragma: public');
$fp = fopen('php://output', 'a');//打开output流


//mb_convert_variables('GBK', 'UTF-8', $str);
fputs($fp, $str);//将数据格式化为CSV格式并写入到output流中



unset($str);//释放变量的内存
ob_flush();
flush();//必须同时使用 ob_flush() 和flush() 函数来刷新输出缓冲。
fclose($fp);

exit;
```
 
 
 
