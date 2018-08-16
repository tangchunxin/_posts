---
title: git服务器搭建!!!
date: 2018-04-09 11:11:55
tags:
---

## 在Linux下搭建 Git 服务器
```python
 环境：
服务器 CentOS6.6 + git（version 1.7.1）
客户端 Windows10 + git（version 2.8.4.windows.1）
```
***


 
### 1.安装 Git
- Linux 做为服务器端系统
- Windows 作为客户端系统，分别安装 Git

#### 服务器端
#yum install -y git
安装完后，查看 Git 版本

```python
[root@localhost ~]# git --version
git version 1.7.1
```

#### 客户端

下载 Git for Windows，地址：https://git-for-windows.github.io/

安装完之后，可以使用 Git Bash 作为命令行客户端。

安装完之后，查看 Git 版本

```python
$git --version
git version 2.8.4.windows.1
```




### 2.服务器端创建 git 用户，用来管理 Git 服务，并为 git 用户设置密码

```python
[root@localhost home]# id git
id: git：无此用户
[root@localhost home]# useradd git
[root@localhost home]# passwd git
```

### 3.服务器端创建 Git 仓库
设置 /home/data/git/gittest.git 为 Git 仓库
然后把 Git 仓库的 owner 修改为 git

```python
[root@localhost home]# mkdir -p data/git/gittest.git
[root@localhost home]# git init --bare data/git/gittest.git
Initialized empty Git repository in /home/data/git/gittest.git/
[root@localhost home]# cd data/git/
[root@localhost git]# chown -R git:git gittest.git/
 ```

### 4.客户端 clone 远程仓库
进入 Git Bash 命令行客户端，创建项目地址（设置在 d:/github）并进入:

复制代码

```python
git@Lenovo-PC MINGW64 /d
$ mkdir github

git@Lenovo-PC MINGW64 /d
$ cd github

git@Lenovo-PC MINGW64 /d/github
$ git clone git@123.57.249.75:/home/data/git/gittest.git
```

如果服务器修改了默认端口22 ,则

```python
git@Lenovo-PC MINGW64 /d/github
$ git clone ssh://git@123.57.249.75:223/home/data/git/gittest.git
 ```

当第一次连接到目标 Git 服务器时会得到一个提示：

```python
The authenticity of host '123.57.249.75 (123.57.249.75)' can't be established.
RSA key fingerprint is SHA256:Ve6WV/SCA059EqoUOzbFoZdfmMh3B259nigfmvdadqQ.
Are you sure you want to continue connecting (yes/no)? 
```

选择 yes：


```python
Warning: Permanently added '123.57.249.75' (RSA) to the list of known hosts.


![](http://images2015.cnblogs.com/blog/534303/201608/534303-20160828224409166-1327160680.png)

此时 C:\Users\用户名\.ssh 下会多出一个文件 known_hosts，以后在这台电脑上再次连接目标 Git 服务器时不会再提示上面的语句。
![](http://images2015.cnblogs.com/blog/534303/201608/534303-20160828225138698-412131557.png)


####  what  known_hosts ? who ?
[ssh2基于口令的安全验证](http://os.51cto.com/art/201209/358478.htm)

### 5.客户端创建 SSH 公钥和私钥

```python
$ ssh-keygen -t rsa -C "1277045182@qq.com"
```

![](http://images2015.cnblogs.com/blog/534303/201608/534303-20160828225309533-1152112221.png)
此时 C:\Users\用户名\.ssh 下会多出两个文件 id_rsa 和 id_rsa.pub

id_rsa 是私钥

id_rsa.pub 是公钥
![](http://images2015.cnblogs.com/blog/534303/201608/534303-20160828225436257-770631205.png)

### 6.服务器端 Git 打开 RSA 认证
进入 /etc/ssh 目录，编辑 sshd_config，打开以下三个配置的注释：

```python
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```
保存并重启 sshd 服务：

```python
[root@localhost ssh]# /etc/rc.d/init.d/sshd restart
或
[root@localhost ssh]# service sshd restart
CentOS7使用
[root@localhost ssh]# systemctl restart sshd.service
``` 

由 AuthorizedKeysFile 得知公钥的存放路径是 .ssh/authorized_keys，实际上是 $Home/.ssh/authorized_keys，由于管理 Git 服务的用户是 git，所以实际存放公钥的路径是 /home/git/.ssh/authorized_keys

在 /home/git/ 下创建目录 .ssh

```python
[root@localhost git]# pwd
/home/git
[root@localhost git]# mkdir .ssh
[root@localhost git]# ls -a 
. .. .bash_logout .bash_profile .bashrc .gnome2 .mozilla .ssh
```

然后把 .ssh 文件夹的 owner 修改为 git

```python
[root@localhost git]# chown -R git:git .ssh
[root@localhost git]# ll -a
总用量 32
drwx------. 5 git  git  4096 8月  28 20:04 .
drwxr-xr-x. 8 root root 4096 8月  28 19:32 ..
-rw-r--r--. 1 git  git    18 10月 16 2014 .bash_logout
-rw-r--r--. 1 git  git   176 10月 16 2014 .bash_profile
-rw-r--r--. 1 git  git   124 10月 16 2014 .bashrc
drwxr-xr-x. 2 git  git  4096 11月 12 2010 .gnome2
drwxr-xr-x. 4 git  git  4096 5月   8 12:22 .mozilla
drwxr-xr-x. 2 git  git  4096 8月  28 20:08 .ssh
```

### 7.将客户端公钥导入服务器端/home/git/.ssh/authorized_keys 文件

回到 Git Bash 下，导入文件：

```python
$ ssh git@192.168.56.101 'cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
```

需要输入服务器端 git 用户的密码
![](http://images2015.cnblogs.com/blog/534303/201608/534303-20160828230957010-2048402011.png)

回到服务器端，查看 .ssh 下是否存在 authorized_keys 文件：

```python
[root@localhost git]# cd .ssh
[root@localhost .ssh]# ll

总用量 4
-rw-rw-r--. 1 git git 398 8月  28 20:08 authorized_keys
```

可以查看一下是否是客户端生成的公钥。

重要：

修改 .ssh 目录的权限为 700

修改 .ssh/authorized_keys 文件的权限为 600

```python
[root@localhost git]# chmod 700 .ssh
[root@localhost git]# cd .ssh
[root@localhost .ssh]# chmod 600 authorized_keys 
```

### 8.客户端再次 clone 远程仓库

```python
$ git clone git@192.168.56.101:/home/data/git/gittest.git
```

![](http://images2015.cnblogs.com/blog/534303/201608/534303-20160828232005724-1805448371.png)
![](/public/imgs/534303-20160828232005724-1805448371.png)
### 9.禁止 git 用户 ssh 登录服务器
之前在服务器端创建的 git 用户不允许 ssh 登录服务器

编辑 /etc/passwd

找到：

```python
git:x:501:501::/home/git:/bin/bash
```

修改为

```python
git:x:501:501::/home/git:/usr/bin/git-shell
```

此时 git 用户可以正常通过 ssh 使用 git，但无法通过 ssh 登录系统。
