---
title: yum安装mysql
date: 2018-09-25 10:25:27
tags:
---

centOS7的yum源中默认好像是没有mysql的。为了解决这个问题，我们要先下载mysql的repo源。
1. 下载mysql的repo源
$ wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
2. 安装mysql-community-release-el7-5.noarch.rpm包
$ sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
安装这个包后，会获得两个mysql的yum repo源：/etc/yum.repos.d/mysql-community.repo，/etc/yum.repos.d/mysql-community-source.repo。
3. 安装mysql
$ sudo yum install mysql-server
根据步骤安装就可以了，不过安装完成后，没有密码，需要重置密码。
4. 重置密码
重置密码前，首先要登录
$ mysql -u root
登录时有可能报这样的错：ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)，原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户：
$ sudo chown -R root:root /var/lib/mysql
然后，重启服务：
$ service mysqld restart
接下来登录重置密码：
正确的root 密码修改：

在mysql 的安装目录中找到 /usr/bin/mysqld_safe 文件， ./mysqld_safe --skip-grant-tables

之后就启动了不用密码的环境：

Mysql -u root

Mysql> update mysql.user set password = password('red') where User='root';

Mysql> flush privileges;

Myusql> quit;
