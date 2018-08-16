---
title: linux nginx设置禁止通过ip访问服务器
date: 2018-04-28 21:35:28
tags:
---
#### 为了避免别人把未备案的域名解析到自己的服务器ip而导致服务器被断网，需要在nginx上设置禁止通过ip访问服务器，只能通过域名访问。  
- 最关键的一点是，在server的设置里面添加这么一行：  
- Listen 80 default;  
- 后面的default参数表示这个是默认的虚拟主机。  
- 例如：别人如果通过ip或者未知域名访问你的网站的时候，你希望禁止显示任何有效内容，可以给他返回404。具体如下：  
```
server {  
       listen 80 default;
       listen [::]:80 default;
       server_name _;  
       return 404;  
}  
```
- 当然，按照上述设置，的确不能让别人通过ip访问服务器了，但是还应该开放一个或多个真实的希望被访问的域名配置，设置如下：  
```
//增加一个server段
server {  
       listen 80 default;
       listen [::]:80 default;
       server_name _;  
       return 404;  
} 

//将原有的 修改为只允许某几个域名访问
server {  
        linten 80; 
        listen [::]:80;
        server_name www.941db.com www.aaa.com； （以世海夺宝网为例）  
………..  
} 
```