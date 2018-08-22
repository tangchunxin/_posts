---
title: fiddle https配置
date: 2018-08-17 17:11:36
tags:
---
## 原blog地址
- https://blog.csdn.net/laofashi2015/article/details/78476499


## 安装之后 app还是抓取不了https包  具体问题是因为

- IOS 10.3升级之后，安装的证书默认是不启用的，需要手动去开启。

- 设置 –> 通用 –> 关于本机 –> 证书信息设置; 将Fiddler的证书开关打开就行了