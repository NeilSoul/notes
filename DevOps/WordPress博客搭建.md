本来想搭一个轻博客（类似ghost），但发现功能太少不好用，例如就没有归档和搜索功能，插件安装也不方便，于是选择了wordpress。wp是最流行的开源博客系统，由php代码书写，使用mysql数据库。

## 安装LNMP(Nginx/MySQL/PHP)环境

Nginx在之前的博文已经有介绍，这里就简单介绍Mysql和PHP的安装。

### 首先是MySQL

Centos7中用MariaDB代替了MySQL数据库，为了安装MySQL，我们先从官网找到MySQL的[rpm源](http://dev.mysql.com/downloads/repo/yum/)，然后通过yum方式来安装。

```bash
# wget http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm
# rpm -ivh mysql57-community-release-el7-7.noarch.rpm
# yum install mysql-server
```

我选择的是MySQL5.7版本，在5.7之后的版本，安装后会生成一个随机的密码，所以我们安装完成后第一步是把密码找到并修改之。

```bash
# grep 'temporary password' /var/log/mysqld.log
MySQL v 5.7 or higher generates a temporary random password after installation and stored that in mysql error log file, located at /var/log/mysqld.log 
for an installation by the MySQL Yum repository on CentOS 7.
```

找到密码之后,用mysqladmin来修改，注意到5.7之前的mysql-safe是已经没有了。

```bash
# mysqladmin -u root password "your_password" -p
密码必须包含大小写字母，特殊字符，数字
```

### 然后是PHP

完整的PHP包含了很多模块，一一安装。

```bash
# yum install php-fpm  php-mysql  php-mbstring php-gd php-pear php-mcrypt  php-mhash php-eaccelerator php-suhosin php-tidy php-curl
```

### LNMP设置

安装好Nginx，MySQL，PHP之后，首先设置他们开启启动，用CentOS7常用的命令systemctl。

```bash
# systemctl enable nginx
# systemctl enable mysqld
# systemctl enable php-fpm
```

启动MySQL，以root用户登录，并创建wordpress相应的表。

```bash
# systemctl start mysqld
# mysql -u root -p
```

创建一个名为wordpress的数据库,给它授权，使用户名为”wpuser”，操作权限是localhost，登录密码是自定义。建表SQL命令如下

```sql
create database wordpress;
grant all on wordpress.* to wpuser@localhost identified by 'your_password_4_mysql';
```

php-fpm配置,默认配合apache，这里改为nginx。

```bash
# vi /etc/php-fpm.d/www.conf
找到以下两行：
user = apache
group = apache
将其中的apache都改为nginx
```

nginx配置，更改网站根目录，添加php默认。

```bash
# vi /etc/nginx/conf.d/default.conf
```

修改两个地方

```
location / {
  root /usr/share/nginx/html;
  index index.php index.html index.htm;
  #启用伪静态规则，可以支持自定义链接和日志别名
  if (!-e $request_filename)
  {
    rewrite ^/(.+)$ /index.php last;
  }
}
```

以及

```
location ~ \.php$ {
  root html;
  fastcgi_pass 127.0.0.1:9000;
  fastcgi_index index.php;
  fastcgi_param SCRIPT_FILENAME /usr/share/nginx/html$fastcgi_script_name;
  include fastcgi_params;
}
```

上述代码是为了添加php-fpm支持，在默认配置中就有，只是被注释掉了，我们更改了其中的网站目录地址。
然后重启nginx和php-fpm。

```bash
# systemctl restart nginx
# systemctl restart php-fpm
```

至此LNMP环境搭建结束。

## 下载wordpress并部署

我选择的是wordpress中文版，从[官网](https://cn.wordpress.org/)获得最新的下载地址。下载并解压wordpress文件至 /usr/share/nginx/html/，并设置权限为711即可，注意修改html目录及其下面的所有文件属主为nginx，组为nginx（使用chown命令即可）。

```bash
# wget https://cn.wordpress.org/wordpress-4.3.1-zh_CN.zip
# unzip wordpress-4.3.1-zh_CN.zip -d /usr/share/nginx/html/
# chown -R nginx /usr/share/nginx/html/
```

注意到如果没有设置html目录及所有文件拥有者为nginx（安装了nginx后，系统中就有这个用户），在运行时会经常提醒权限不够。

## 补充

### 中文目录分类，标签的404问题

在wp-includs文件夹下面，找到rewrite.php文件。用编辑器打开，搜索找到function get_extra_permastruct($name) ，代码如下：

```
function get_extra_permastruct($name) { 
  if ( empty($this->permalink_structure) ) return false; 
  if ( isset($this->extra_permastructs[$name]) ) return $this->extra_permastructs[$name][0]; 
  return false; 
}
```

对这段代码进行修改，添加个英文”!”即可，改为如下形式：

```
function get_extra_permastruct($name) { 
  if ( !empty($this->permalink_structure) ) return false; 
  if ( isset($this->extra_permastructs[$name]) ) return $this->extra_permastructs[$name][0]; 
  return false; 
}
```

### SeLinux设置

之前在Nginx安装的那篇文章中，我提到要关闭防火墙和SeLinux，其实更好的方法是

```bash
# setsebool -P httpd_enables_homedirs=1  
用于设置selinux权限，表示允许用户通过httpd访问www文件夹，这个权限很重要。
```

### Nginx下WordPress伪静态规则设置

打开nginx配置文件：

```bash
# vim /etc/nginx/conf.d/default.conf（此路径根据Linux版本与安装路径会有不同）
```

在server容器中添加下面这几行

```
location /
{
try_files $uri $uri/ /index.php?q=$uri&args;
}
```