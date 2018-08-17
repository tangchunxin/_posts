---
title: sql关闭 query_cache
date: 2018-08-17 17:12:59
tags:
---

## MySQL query_cache_type 详解
- set global query_cache_size=0;
- set global query_cache_type=0;

--------

#### MySQL设置查询缓存的用意：

　　把查询到的结果缓存起来，下次再执行相同查询时就可以直接从结果集中取；这样就比重新查一遍要快的多。

 

查询缓存的最终结果是事与愿违：

　　之所以查询缓存并没有能起到提升性能的做用，客观上有如下两点原因

　　1、把SQL语句的hash值作为键，SQL语句的结果集作为值；这样就引起了一个问题如 select user from mysql.user 和 SELECT user FROM mysql.user 

　　这两个将会被当成不同的SQL语句，这个时候就算结果集已经有了，但是一然用不到。

 

　　2、当查询所基于的低层表有改动时与这个表有关的查询缓存都会作废、如果对于并发度比较大的系统这个开销是可观的；对于作废结果集这个操作也是要用并发

　　访问控制的，就是说也会有锁。并发大的时候就会有Waiting for query cache lock 产生。

 

　　3、至于用不用还是要看业务模型的。

 

如果何配置查询缓存：

　　query_cache_type 这个系统变量控制着查询缓存工能的开启的关闭。

　　query_cache_type=0时表示关闭，1时表示打开，2表示只要select 中明确指定SQL_CACHE才缓存。

　　这个参数的设置有点奇怪，1、如果事先查询缓存是关闭的然而用 set @@global.query_cache_type=1; 会报错

　　ERROR 1651 (HY000): Query cache is disabled; restart the server with query_cache_type=1 to enable it

　　2、如果事先是打开着的尝试去闭关它，那么这个关闭也是不完全的，这种情况下查询还是会去尝试查找缓存。

　　最好的关闭查询缓存的办法就是把my.cnf 中的query_cache_type=0然后再重启mysql。

 

查询缓存相关的系统变量：

　　have_query_cache　　表示这个mysql版本是否支持查询缓存。

　　query_cache_limit　　 表示单个结果集所被允许缓存的最大值。

　　query_cache_min_res_unit　　每个被缓存的结果集要占用的最小内存。

　　query_cache_size　　用于查询缓存的内存大小。

 

如何监控查询缓存的命中率： 

　　Qcache_free_memory　　查询缓存目前剩余空间大小。

　　Qcache_hits　　　　　     查询缓存的命中次数。

　　Qcache_inserts　　　　　 查询缓存插入的次数。

　　也就是说缓存的命中率为 Qcache_hits/(Qcache_hits+Qcache_inserts)