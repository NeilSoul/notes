CentOS7安装Nginx
================
**Nginx**是一款轻量级的Web服务器/反向代理服务器及电子邮件(IMAP/POP3)代理服务器。例如你起了一个NodeJS服务，端口号是3333，可以用nginx映射3333到80端口，就可以直接通过浏览器访问默认端口的url。注意到1024以下的端口是需要root权限的。

## 系统环境
CentOS7

## 安装方式
在centos下，最方便的安装方式是yum，但没有nginx的源，需要先从[Nginx官网][1]下载已编译包（package），然后更新源。

### 下载对应当前系统的nginx包
```sh
wget http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```
### 建立nginx的yum仓库,需要root权限
```sh
sudo rpm rpm -ivh nginx-release-centos-7-0.el7.ngx.noarch.rpm
```
### yum方式安装nginx，需要root权限
```sh
sudo yum install nginx
```
### 管理nginx服务
```sh
# 控制面板
systemctl start nginx
systemctl status nginx
systemctl stop nginx
systemctl restart nginx
# 开机启动
systemctl enable nginx
```
### 测试
使用启动命令，在浏览器输入服务器地址，会看到`welcome togninx！`的字样。

### 配置
默认配置文件在路径`/etc/nginx`下，可以修改目录里的`nginx.conf`来实现自定义。
[1]: http://nginx.org/
