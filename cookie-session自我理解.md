---
title: cookie_session自我理解
date: 2018-04-23 22:29:02
tags:
---

## cookie 和session 自我理解
 开始 PHP Session
 在您把用户信息存储到 PHP session 中之前，首先必须启动会话。
 注释：session_start() 函数必须位于 <html> 标签之前：
 存储和取回 session 变量的正确方法是使用 PHP $_SESSION 变量：
```
<?php
session_start();
// 存储 session 数据
$_SESSION['views']=1;
?>
```

#### 流程
- 浏览器请求服务器上一个test.php文件

- test.php执行开启session_start();

- 这个时候服务器给浏览器的 返回头信息中  包含着一句Set_Cookie,这句表示告诉浏览器 写一个名为PHPSESSID的cookie,并在服务器上生成一个文件,存放这个session文件
```
Response Header
Access-Control-Allow-Headers: DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type
Access-Control-Allow-Methods: GET,POST,OPTIONS
Access-Control-Allow-Origin: *
Cache-Control: must-revalidate, no-store
Connection: keep-alive
Content-Encoding: gzip
Content-Type: image/png
Date: Mon, 23 Apr 2018 13:00:35 GMT
Pragma: no-cache
Server: nginx/1.10.2
Set-Cookie: PHPSESSID=uorfbpp8m2prv9olfpa6vmelie; path=/==
Transfer-Encoding: chunked
Vary: Accept-Encoding
X-Daa-Tunnel: hop_count=1
X-NWS-LOG-UUID: c460b76b-08e6-4a6c-8fc0-4ffd7c0a2a8f af9b19c3b29c523d7d5bf5e81938f9cd
X-Powered-By: PHP/7.1.4
```

- 此时Cookies中便存在cookie

Name|Value|
---|:---:|
PHPSESSID|uorfbpp8m2prv9olfpa6vmelie

- 这个是浏览器每次访问服务器的时候在请求头信息中带有Cookie: PHPSESSID=uorfbpp8m2prv9olfpa6vmelie
```
Request Header
Accept: */*
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: keep-alive
Cookie: PHPSESSID=uorfbpp8m2prv9olfpa6vmelie
Host: test.linfiy.com
Referer: http://test.linfiy.com/mahjong/client/backstage-cangzhou/
User-Agent: Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Mobile Safari/537.36
```

- cookie 如果设置了有效期 setcookie("name","key",time()+3600); 同时浏览器关不清除cookie则  关闭浏览器cookie在有效期内不失效;cookie写的是过去的时间戳,不是多少秒;setcookie设置到那个时间段过期
- ini_set('session.gc_maxlifetime', "3600"); // 默认1440秒 也就是24分钟 ,多少秒有效
- ini_set("session.cookie_lifetime","3600"); // 秒
- 
今天想和大家分享一个关于Session的话题: 当浏览器关闭时，Session就被销毁了？

我们知道Session是JSP的九大内置对象（也叫隐含对象）中的一个，它的作用是可以保

存当前用户的状态信息，初学它的时候，认为Session的生命周期是从打开一个浏览器窗

口发送请求到关闭浏览器窗口，但其实这种说法是不正确的！下面就具体的去解释：

当用户第一次访问Web应用中支持Session的某个网页时，就会开始一个新的Session，

那么接下来当用户浏览这个Web应用的不同网页时，始终处于一个Session中

再详细些：

当一个Session开始时，Servlet容器会创建一个HttpSession对象，那么在HttpSession对象中，可以存放用户状态的信息

Servlet容器为HttpSession对象分配一个唯一标识符即Sessionid，Servlet容器把Sessionid作为一种Cookie保存在客户端的 *浏览器* 中

用户每次发出Http请求时，Servlet容器会从HttpServletRequest对象中取出Sessionid,然后根据这个Sessionid找到相应的HttpSession对象，从而获取用户的状态信息
以上就是Session的运行机制，但是还没有提到Session的生命周期，再往下了解！

其实让Session结束生命周期，有以下两种办法：

一个是Session.invalidate()方法，不过这个方法在实际的开发中，并不推荐，可能在强制注销用户的时候会使用；
一个是当前用户和服务器的交互时间超过默认时间后，Session会失效
我们知道Session是存在于服务器端的，当把浏览器关闭时，浏览器并没有向服务器发送

任何请求来关闭Session，自然Session也不会被销毁，但是可以做一点努力，在所有的

客户端页面里使用js的window.onclose来监视浏览器的关闭动作，然后向服务器发送一

个请求来关闭Session，但是这种做法在实际的开发中也是不推荐使用的，最正常的办法

就是不去管它，让它等到默认的时间后，自动销毁

那么为什么当我们关闭浏览器后，就再也访问不到之前的session了呢？

其实之前的Session一直都在服务器端，而当我们关闭浏览器时，此时的Cookie是存在

于浏览器的进程中的，当浏览器关闭时，Cookie也就不存在了。

其实Cookie有两种:

一种是存在于浏览器的进程中;
一种是存在于硬盘上
而session的Cookie是存在于浏览器的进程中，那么这种Cookie我们称为会话Cookie，

当我们重新打开浏览器窗口时，之前的Cookie中存放的Sessionid已经不存在了，此时

服务器从HttpServletRequest对象中没有检查到sessionid，服务器会再发送一个新的存

有Sessionid的Cookie到客户端的浏览器中，此时对应的是一个新的会话，而服务器上

原先的session等到它的默认时间到之后，便会自动销毁。

ps:

当在同一个浏览器中同时打开多个标签，发送同一个请求或不同的请求，仍是同一个session;

当不在同一个窗口中打开相同的浏览器时，发送请求，仍是同一个session;

当使用不同的浏览器时，发送请求，即使发送相同的请求，是不同的session;

当把当前某个浏览器的窗口全关闭，再打开，发起相同的请求时，就是本文所阐述的，是不同的session,但是它和session的生命周期是没有关系的.