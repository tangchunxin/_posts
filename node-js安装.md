---
title: node.js安装
date: 2018-04-25 21:39:35
tags:
---
 
###　首先下载安装包

- https://nodejs.org/en/download/
- 
点击下载相应的zip版本

![](https://images2018.cnblogs.com/blog/1028599/201712/1028599-20171225194706650-1436753993.png)

- 然后将文件夹解压到任意目录

- 比如我这里解压到了：C:\Program Files\nodejs中

- 然后在这个目录下新建两个文件夹
```
node-cache
node-global
```
ps:这是用来放npm全局模块的安装目录，也可以放到其他地方。

 

### 配置环境变量

- 编辑系统Path变量
```
变量名：Path
变量值（你的安装目录）：C:\Program Files\nodejs
```

- 编辑Path变量
新增两个条目
%NODE_HOME%
%NODE_HOME%\node-global
```
变量名：Path
变量值（你的安装目录）：%NODE_HOME%;%NODE_HOME%\node-global
```
 

### 打开CMD，如果执行的时候报错，试着换用管理员运行

- 运行下面的命令，这里的地址也是安装目录
```
npm config set prefix "C:\Program Files\nodejs\node-global"
npm config set cache "C:\Program Files\nodejs\node-cache"
 
```
### 测试，运行下面的命令有版本出现即可
```
node -v
npm -v
```

## hexo
https://zhuanlan.zhihu.com/p/26625249