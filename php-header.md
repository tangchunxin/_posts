---
title: php  header
date: 2018-04-28 21:30:57
tags:
---
- header("Location:/index.php");//跳转到当前域名下的文件
- header("Location:https://www.baidu.com");//跳转到其他域名
- @header("http/1.1 404 not found"); //跳转到404页面
- "http://".$_SERVER['SERVER_NAME'].$_SERVER['REQUEST_URI'];//当前请求url,例如http://www.p2peye.com/test.php?a=1