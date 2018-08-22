---
title: mysql ignore插入去重复
date: 2018-08-22 22:44:07
tags:
---
#### Mysql 唯一索引 防止重复插入数据

一般的批量插的 sql语句数据违反唯一性约束时，出现重复数据将会直接报错并停止执行

insert into tb_name (field1,field2) values(f11,f12),(f21,f22)...

这种语句将会报错并停止执行   Warning: (1062, "Duplicate entry '  ' for key '索引'")

#### 解决方法：

在语句中添加 ignore 关键字

insert ignore into tb_name (field1,field2) values(f11,f12),(f21,f22)...

这个语句数据违反唯一性约束时，出现重复数据则会将会直接跳过