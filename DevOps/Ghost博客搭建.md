
## 系统环境

CentOS.7

## 预处理

 1. 安装NodeJS，注意版本必须是0.10.*~0.12.*
 2. 安装Nginx

## 安装ghost

参考官网的步骤

## 通过forever来后台启动nodejs服务

当我们用ssh或者putty登录远程服务器时，一旦关闭窗口（close session），则所有我们当前执行的进程也会关闭。很多时候我们需要将web服务这样的进程转入后台执行，从而不会随着session关闭而关闭。Linux系统下，**nohup**或**screen**控制后台执行的两个不错的方法，而我将介绍的forever则是nodejs的一个工具，可以管理后台nodejs进程。

安装forever

```sh
# npm install forever -g
```

使用

```sh
$ forever start [app.js]
启动
$ forever list
查看进程
$ forever stop [app.js]
关闭进程
```

具体启动ghost的命令如下，首先进入ghost的目录

```sh
$ NODE_ENV=production forever start index.js
需要设置一下node的环境，让它执行production版本，不然默认development
```

## 配置nginx
查看了nginx.conf文件，发现它会包含所有路径`/etc/nginx/conf.d`下的配置文件。可以参考原始该目录下的`default.conf`。
首先

```sh
$ mv default.conf default.conf.bak
去掉默认配置
$ vi ghost.conf
添加ghost配置
```

把以下ghost的配置代码添加进去

```
upstream ghost {
    server 127.0.0.1:2368;
    keepalive 64;
}

server {
    listen 80;
    server_name adai.today;
    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host  $http_host;
        proxy_set_header X-Nginx-Proxy true;
        proxy_set_header Connection "";
        proxy_pass      http://ghost;

    }

}
```

然后重启nginx服务
```sh
$ systemctl restart nginx
```

## 问题
在CentOS7环境下，跑**Nodejs**和**Nginx**一套框架，从外网访问时，总会遇到一些问题，诸如502错误，网页无法访问等。经常是因为

### 防火墙
centos默认防火墙是开启的，会禁用一些端口（例如ghost常用的2368），从而无法从外网访问。目前用的方法是关闭之

```sh
# Disable Firewalld Service.
systemctl disable firewalld
# Stop Firewald Service
systemctl stop firewalld
# Start Firewald Service
systemctl start firewalld
```

### SeLinux
这个服务也是默认开启的，它会让nginx代理端口转发的时候产生权限问题，从而`connect failed`

```sh
# 查看SeLinux状态
sestatus
# 临时关闭
setenforce 0
# 永久关闭
vi /etc/selinux/config
# 将`SELINU`置为`disabled`
```

## 去掉Google Fonts
Ghost博客默认的主题加载了Google Fonts，导致博客打开变慢或者根本上打不开，解决的办法就是去掉主题中加载的Google Fonts链接。
 1. Ghost博客后台去掉Google Fonts需要进入到：core/server/views/default.hbs和core/server/views/user-error.hbs，把里面的fonts.googleapis.com链接删除了。
 2. 默认的主题去掉Google Fonts需要进入到：content/themes/casper/default.hbs，把里面的fonts.googleapis.com链接删除了。

## 博客备份与恢复
Ghost 博客的所有文章内容都是存储在 sqlite3 数据库中的，其位置是 /content/data/ghost.db。另外，所有上传的图片都放在了 /content/images/ 目录下。
Ghost博客自带了一个备份与恢复的页面，地址是：域名/ghost/debug/。 点击 Export 按钮就可以将博客内容导出为 .json 文件，还有一个导入工具 Import ，可以将 .json 格式的备份内容导入Ghost 系统。 最后一个红色按钮 Delete all content 是用来删除所有内容（即清空数据库）。


