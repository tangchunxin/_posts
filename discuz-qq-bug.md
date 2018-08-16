---
title: discuz_qq_bug
date: 2018-04-18 22:54:59
tags:
---

## 今天在工作中领导交给我一个bug
## Discuz! X3.2QQ登录出现redirect uri is illegal(100010)的解决办法

|详解: 如果同步站点信息时使用站点URL：bbs.xxx.com，那么在其他域名如：www.xxx.com登录，也就是同一主域名下的其它二级域名登录，那么就会出现：redirect uri is illegal(100010)。


|原因是腾讯在4月末更改了QQ互联的规则，在不同的二级域名是无法通过QQ登录时的授权的。


|用TX的话说“就是指你的回调地址不合法”

### 解决办法
- 打开：source\plugin\qqconnect\connect.class.php
- 找到

```
$_G['siteurl']    //获取当前域名
```
修改为:

```
http://你的网站/
```
