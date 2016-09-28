---
title: Linux 安装 MySQL
date: 2016-09-09 18:33:38
tags:
    - MySQL
---

## 定义
在Linux服务器上安装MySQL数据库
<!-- more -->
## 登录Linux服务器
```
ssh root@xxx.xxx.xxx.xx
```

## 下载mysql的repo源
```
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
```

## 安装mysql-community-release-el7-5.noarch.rpm包
```
rpm -ivh mysql-community-release-el7-5.noarch.rpm
```

## 安装mysql
```
yum install mysql-server
```

## 登录mysql
```
mysql -u root
```
登录时有可能报这样的错：ERROR 2002 (HY000): Can‘t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock‘ (2)，原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户：
```
sudo chown -R openscanner:openscanner /var/lib/mysql
```
然后重启服务
```
service mysql restart
```

## 登录重置密码
```
use mysql;

update user set password=password('123456') where user='root';

flush privileges

exit
```

## 如果有防火墙的话要开启3306端口，没有限制端口就不需要，这样就可以远程连接
看一下防火墙情况，如果有2行就是开了
```
ps aux|grep iptable
ps aux|grep firewalld

如果没有开
sudo vim /etc/sysconfig/iptables

//添加
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
//保存

// 重启防火墙
sudo service iptables restart

// 查看端口
netstat -antlp | grep 3306
// tcp6       0      0 :::3306                 :::*                    LISTEN      3050/mysqld
// tcp6 就是ok了
```

## 基础命令
- mysql -u root 登录
- mysql use 进入
- show databases
- show tables
- exit 退出