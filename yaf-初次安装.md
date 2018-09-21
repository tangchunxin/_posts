---
title: yaf 初次安装
date: 2018-09-21 17:49:01
tags:
---

> 1 .https://blog.csdn.net/aozeahj/article/details/62445959

## yaf扩展安装
http://www.zixuephp.net/article-383.html


## 下载yaf项目：

在github上

搜索yaf

复制zip链接

https://github.com/laruence/yaf/archive/master.zip

进入自己目录

mkdir tmp   //新建一个临时目录

cd tmp/

ls

wget https://github.com/laruence/yaf/archive/master.zip


unzip master.zip //解压
会出现一个yaf-master目录
cd yaf-master

cd tools/

cd cg/

在这个目录执行

：./yaf_cg   //会生产yaf最简单代码

：./yaf_cg itbull 取个名字

注意：//这里会出现三个命令没打开，是因为要在php.ini  disable_functions把这几个删掉

vim /usr/local/php/etc/php.ini

cd output/

ls

itbull这里面就是刚刚生产的代码

cd /home/work/itbull/我们项目目录

cp -rf tmp/yaf-master/tools/cg/output/itbull/* ./复制那些代码



进入项目目录，就可以了！


## 路由
https://blog.csdn.net/Cpath/article/details/69218478



## 修改路由配置
\application\Bootstrap.php

```
public function _initRoute(Yaf_Dispatcher $dispatcher) {
		//在这里注册自己的路由协议,默认使用简单路由
        $router = $dispatcher->getRouter();
        $router->addConfig(Yaf_Registry::get("config")->routes);
	}
	
	
```

\conf\application.ini
```

[common]
application.directory = APPLICATION_PATH  "/application"
application.dispatcher.catchException = TRUE



;添加一个名为simple的路由协议
routes.simple.type="simple"
routes.simple.controller=c
routes.simple.module=m
routes.simple.action=a
;添加一个名为supervar的路由协议
routes.supervar.type="supervar"
routes.supervar.varname=r
[product : common]

```
	

## rewrite

rewrite ^([^\.]*)/(.+)/(.+)$  $1/index.php?r=/$2/$3 last;

## 访问方式

www.xxx.com/index/test1?get=参数



