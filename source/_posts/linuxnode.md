---
title: Linux服务器的node环境配置
date: 2016-09-09 10:22:15
tags:
    - Linux
---

## 目的
在Linux云服务器上配置node环境
<!-- more -->

## 登录虚拟机
购买服务器，获取到公网IP地址，使用ssh登录，并输入root密码
```
ssh root@xxx.xxx.xxx.xx
```

## 添加公钥
目的是下次通过ssh登录服务器时候不需要密码
把本机获取公钥 **cat ~/.ssh/id_rsa.pub**，并粘贴进去。
```
vim ~/.ssh/authorized_keys
```

## node

### 创建存放文件夹
创建一个文件夹用于存放下载文件
```
cd / && mkdir svr-setup && cd svr-setup
```

### 下载node源码包
```
wget https://nodejs.org/dist/v4.4.7/node-v4.4.7.tar.gz
```

### 解压node源码包
```
tar zxvf node-v4.4.7.tar.gz
```

### 安装开发包工具
用于解析源码包
```
yum -y group install "Development Tools"
```

### 安装node
#### 生成makefile
```
cd node-v4.4.7 && ./configure
```

#### 解析
```
make
```

#### 安装
```
make install
```

#### 检验
查看node版本号确认安装node环境完毕
```
node -v
```

## Code

### 创建wwwroot用于存放代码库
```
cd /data && mkdir wwwroot && cd wwwroot
```

### 创建test代码库
```
mkdir test && cd test
```

### 创建test.html 
测试文件并编辑。
```
touch test.html && vim test.html
```

```
hello
```

## Nginx

### 安装nginx
```
yum install nginx
```

### 开启nginx服务
```
start  nginx.service
```

### 检查
产看nginx配置文件地址
```
nginx -t
>>nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
>>nginx: configuration file /etc/nginx/nginx.conf test is successful
```

### 修改配置公共文件
把nginx配置文件中的server部分删除
```
vim /etc/nginx/nginx.conf
```

### 配置
创建test.wildgou.com 的nginx配置文件
```
cd /etc/nginx/conf.d/ && touch test.wildgou.com.conf && vim test.wildgou.com.conf
```

```
server {
listen 80;
        server_name test.wildgou.com;

        root /data/wwwroot/test;

        location / {
                #autoindex on;

                #autoindex_localtime on;

                #try_files $uri $uri/ /index.php;
                #try_files $uri $uri/ /index.php?$query_string;
        }
}
```

### 测试
测试一下看看有没问题，没有问题就可以重启
```
nginx -t
```

### 重启
重启nginx
```
nginx -s reload
```

### 浏览器查看
#### hosts
本机配置hosts
```
sudo vim /etc/hosts
123.207.148.60  test.wildgou.com
```

#### 浏览器打开
浏览器打开这个地址就可以看到自己的测试文件内容，hello。
http://test.wildgou.com/test.html

## nginx配置多个地址
### 生成公钥和密钥
```
ssh-keygen
```

### 链接仓库
查看公钥并且添加到你的bitbucket或者github
```
cat ~/.ssh/id_rsa.pub
```

### clone code
把仓库代码克隆下来
```
cd /data/wwwroot/
git clone git@bitbucket.org:zhuayu/backstage.git
```

### install
安装依赖包
```
npm install
```

### pm2
全局安装pm2,用于管理和重启服务器
```
npm install pm2 -g
```

### 启动
跑起应用
```
pm2 start bin/www
```

### 配置nginx
创建 backstage的nginx配置文件
```
cd /etc/nginx/conf.d/ && touch backstage.wilddog.com.conf
vim backstage.wilddog.com.conf
```

```
server {
listen 80;
        server_name backtage.wildgou.com;

        location / {
proxy_pass http://127.0.0.1:3000;
        }
}
```

```
nginx -t
nginx -s reload
```
