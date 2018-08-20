---
title: Nginx URL（rewrite）介绍
date: 2018-04-28 21:36:29
tags:
---
## Nginx URL重写（rewrite）介绍
```
rewrite ^([^\.]*)/([0-9]+)/([0-9]+)/([0-9]+)/([a-zA-Z0-9]+)\.(gif|jpg|jpeg)$  https://www.baidu.com last;
```
 注意这里要用‘’单引号引起来，避免{}
 ```
rewrite '^/images/([a-z]{2})/([a-z0-9]{5})/(.*)\.(png|jpg|gif)$' /data?file=$3.$4;
```

- 和apache等web服务软件一样，rewrite的组要功能是实现RUL地址的重定向。Nginx的rewrite功能需要PCRE软件的支持，即通过perl兼容正则表达式语句进行规则匹配的。默认参数编译nginx就会支持rewrite的模块，但是也必须要PCRE的支持
-  rewrite是实现URL重写的关键指令，根据regex（正则表达式）部分内容，重定向到replacement，结尾是flag标记。

- rewrite语法格式及参数语法说明如下:

    rewrite

    关键字：其中关键字error_log不能改变

    正则：perl兼容正则表达式语句进行规则匹配

    替代内容：将正则匹配的内容替换成replacement

    flag标记：rewrite支持的flag标记

 

flag标记说明：

last  #本条规则匹配完成后，继续向下匹配新的location URI规则

break  #本条规则匹配完成即终止，不再匹配后面的任何规则

redirect  #返回302临时重定向，浏览器地址会显示跳转后的URL地址

permanent  #返回301永久重定向，浏览器地址栏会显示跳转后的URL地址

rewrite参数的标签段位置：
server,location,if

例子：
rewrite ^/(.*) http://www.czlun.com/$1 permanent;

说明：                                        

rewrite为固定关键字，表示开始进行rewrite匹配规则

regex部分是 ^/(.*) ，这是一个正则表达式，匹配完整的域名和后面的路径地址

replacement部分是http://www.czlun.com/$1 $1，是取自regex部分()里的内容。匹配成功后跳转到的URL。

flag部分 permanent表示永久301重定向标记，即跳转到新的 http://www.czlun.com/$1 地址上

regex 常用正则表达式说明
字符

描述

\

将后面接着的字符标记为一个特殊字符或一个原义字符或一个向后引用。如“\n”匹配一个换行符，而“\$”则匹配“$”

^

匹配输入字符串的起始位置

$

匹配输入字符串的结束位置

*

匹配前面的字符零次或多次。如“ol*”能匹配“o”及“ol”、“oll”

+

匹配前面的字符一次或多次。如“ol+”能匹配“ol”及“oll”、“oll”，但不能匹配“o”

?

匹配前面的字符零次或一次，例如“do(es)?”能匹配“do”或者“does”，"?"等效于"{0,1}"

.

匹配除“\n”之外的任何单个字符，若要匹配包括“\n”在内的任意字符，请使用诸如“[.\n]”之类的模式。

(pattern)

匹配括号内pattern并可以在后面获取对应的匹配，常用$0...$9属性获取小括号中的匹配内容，要匹配圆括号字符需要\(Content\)

rewrite 企业应用场景
Nginx的rewrite功能在企业里应用非常广泛：

u 可以调整用户浏览的URL，看起来更规范，合乎开发及产品人员的需求。

u 为了让搜索引擎搜录网站内容及用户体验更好，企业会将动态URL地址伪装成静态地址提供服务。

u 网址换新域名后，让旧的访问跳转到新的域名上。例如，访问京东的360buy.com会跳转到jd.com

u 根据特殊变量、目录、客户端的信息进行URL调整等

Nginx配置rewrite过程介绍
（1）创建rewrite语句
vi conf/vhost/www.abc.com.conf

#vi编辑虚拟主机配置文件

文件内容

server {

        listen 80;

        server_name abc.com;

        rewrite ^/(.*) http://www.abc.com/$1 permanent;

}

 

 

server {

        listen 80;

        server_name www.abc.com;

        location / {

                root /data/www/www;

                index index.html index.htm;

        }

        error_log    logs/error_www.abc.com.log error;

        access_log    logs/access_www.abc.com.log    main;

}

或者

server {

        listen 80;

        server_name abc.com www.abc.com;

        if ( $host != 'www.abc.com'  ) {

                rewrite ^/(.*) http://www.abc.com/$1 permanent;

        }

        location / {

                root /data/www/www;

                index index.html index.htm;

        }

        error_log    logs/error_www.abc.com.log error;

        access_log    logs/access_www.abc.com.log    main;

}

（2）重启服务
确认无误便可重启，操作如下：

nginx -t

#结果显示ok和success没问题便可重启

nginx -s reload

（3）查看跳转效果
打开浏览器访问abc.com

页面打开后，URL地址栏的abc.com变成了www.abc.com说明URL重写成功。

 ## nginx location rewrite匹配顺序
 
 Rewrite（ URL 重写）指令可以出现在 server{} 下，也可以出现在 location{} 下，它们之间是有区别的！对于出现在 server{} 下的 rewrite 指令，它的执行会在 location 匹配之前；对于出现在 location{} 下的 rewrite 指令，它的执行当然是在 location 匹配之后，但是由于 rewrite 导致 HTTP 请求的 URI 发生了变化，所以 location{} 下的 rewrite 后的 URI 又需要重新匹配 location ，就好比一个新的 HTTP 请求一样（注意由 location{} 内的 rewrite 导致的这样的循环匹配最多不超过 10 次，否则 nginx 会报 500 错误）。总的来说，如果 server{} 和 location{} 下都有 rewrite ，依然是先执行 server{} ，然后进行 location 匹配，如果被匹配的 location{} 之内还有 rewrite 指令，那么继续执行 rewrite ，同时因为 location{} 内的 rewrite 改变了 URI ，那么重写后的结果 URI 需要当做一个新的请求，重新匹配 location （应该也包括重新执行 server{} 下的 rewrite 吧）。