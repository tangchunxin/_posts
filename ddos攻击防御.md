---
title: ddos攻击防御
date: 2018-04-16 22:29:45
tags:
---


## 1、安装DDoS deflate
```
cd /root
wget http://www.inetbase.com/scripts/ddos/install.sh   //下载DDoS deflate
chmod 0700 install.sh      //添加权限
sh install.sh        //执行
```
## 2、配置DDoS deflate下面是DDoS deflate的默认配置位于/usr/local/ddos/ddos.conf ，内容如下：
```
cd  /usr/local/ddos
vim ddos.conf
```
## 默认配置
##### Paths of the script and other files
```
PROGDIR="/usr/local/ddos"
PROG="/usr/local/ddos/ddos.sh"
IGNORE_IP_LIST="/usr/local/ddos/ignore.ip.list"  //IP地址白名单 
CRON="/etc/cron.d/ddos.cron"    //定时执行程序 
APF="/etc/apf/apf"
IPT="/sbin/iptables" 
##### frequency in minutes for running the script
##### Caution: Every time this setting is changed, run the script with --cron
#####            option so that the new frequency takes effect
FREQ=1   //检查时间间隔，默认1分钟 
##### How many connections define a bad IP? Indicate that below.
NO_OF_CONNECTIONS=50     //最大连接数，超过这个数IP就会被屏蔽，一般默认即可 
##### APF_BAN=1 (Make sure your APF version is atleast 0.96)
##### APF_BAN=0 (Uses iptables for banning ips instead of APF)
APF_BAN=0        //使用APF还是iptables。推荐使用iptables,将APF_BAN的值改为0即可。 
##### KILL=0 (Bad IPs are'nt banned, good for interactive execution of script)
##### KILL=1 (Recommended setting)
KILL=1   //是否屏蔽IP，默认即可 
##### An email is sent to the following address when an IP is banned.
##### Blank would suppress sending of mails
EMAIL_TO="root"   //当IP被屏蔽时给指定邮箱发送邮件，推荐使用，换成自己的邮箱即可 
##### Number of seconds the banned ip should remain in blacklist.
BAN_PERIOD=6000    //禁用IP时间，默认6000秒，可根据情况调整
```
## 3 修改配置 问题
```
vim ddos.sh 
1，ip地址过滤的不够细致
在117行，更还此段代码为：
netstat -ntu | awk '{print $5}' | egrep -o "[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}.[0-9]{1,3}" | sort | uniq -c | sort -nr > $BAD_IP_LIST

2，注释掉134行：
echo $CURR_LINE_IP >> $IGNORE_IP_LIST
```
## 4 按照配置文件创建一个执行计划
```
/usr/local/ddos/ddos.sh -c    //按照配置文件创建一个执行计划
http://blog.chinaunix.net/uid-10449864-id-3300661.html
http://czmmiao.iteye.com/blog/1616837
```