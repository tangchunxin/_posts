---
title: sql通配符
date: 2018-04-25 21:42:47
tags:
---
- 在discuz_database类中的public static function query方法
- self::checkquery($sql);在这后面加上 writelog("sqllog",$sql);即可看到所有SQL
- $logdir = DISCUZ_ROOT.'./data/log/sqllog';

## sql通配符
 - SELECT * FROM %t =>查询表t开头的 例如t_common_friendlink 表
 - $sql = 'WHERE (`type` & %s > 0)'  =>SELECT * FROM t_common_friendlink WHERE (`type` & '8' > 0) ORDER BY displayorder
```

二进制

他使用这一个 type  能存储多个类型

比如 type 是  二进制 1111  
它和  8  4  2  1  都能位与 > 0

它就同时属于  1  2  4  8   这四种类型

就是用每个二进制位   置为1 表示一个类型

但是这个在数据库 索引不起作用 数据庞大 这样做就是崩溃了
```
- 在discuz_database类中的public static function query方法
- self::checkquery($sql);在这后面加上 writelog("sqllog",$sql);即可看到所有SQL
- $logdir = DISCUZ_ROOT.'./data/log/sqllog';