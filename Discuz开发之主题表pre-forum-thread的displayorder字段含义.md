---
title: Dz forum_thread的displayorder
date: 2018-05-09 21:54:58
tags:
---

## Discuz开发之主题表pre_forum_thread的displayorder字段含义

#### Discuz!本文就介绍一下pre_forum_thread的displayorder字段含义：

displayorder取值范围为：4,3,2,1,0，-1，-2，-3，-4，他们的含义如下：

- displayorder=4 ：多版块置顶功能可让一个主题在任意多个版块 
```
只在指定的板块显示主题

需要在后台-内容-主题-板块/群组指定来进行操作

以下三种置顶可以在版主管理的浮动窗口操作 
```
- displayorder=3 ：全局置顶 
```
全部专区，每个板块都可已看的到
```

- displayorder=2 ：分类置顶  
```
本专区的所有板块都可以看到该帖
```
- dispalyorder=1 ：本版置顶  
```
只有本板块在置顶区看得到，该板块子版块和其他都看不到
```
- displayorder=0 ：普通贴

#### 在前后台的版主管理操作中，可以进行删除主题的操作，如果此版块设置启用了回收站，那么主题被删除时被放入回收站，displayorder为-1，否则直接从数据库中删除；

```
版块开启回收站：后台-论坛-版块管理-帖子选项
```
- displayorder=-1 ：回收站

- displayorder=-2 ：审核中

- displayorder=-3 ：审核忽略

- displayorder=-4 ：草稿