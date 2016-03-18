在之前关于部署Nginx和博客的文章中，多次提到防火墙和Selinux的设置，安全管理是一个服务器重中之重的问题。

## 环境
 - CentOS 7.0
 - Firewalld是CentOS7.0默认的防火墙软件

## Example
在之前的博文中，用关闭防火墙的方法来使外网可以访问到http，其实不推荐这样做，有很多安全问题，而且系统重启后还得再次关闭防火墙。推荐设置防火前的方法：
```bash
# 在永久配置中 添加http服务
firewall-cmd --permanent --add-service=http
#在不改变状态的条件下重新加载防火墙
firewall-cmd --reload
```
如果是别的端口号，则需要另外配置（添加3000/tcp之类的)。另外SeLinux在enforcing模式时，nginx开机无法启动，无奈之下设置SeLinux禁止开机启动。

## Systemctl
控制面板，可以用来开启和关闭服务，也可以设置开机启动
```bash
systemctl enable [service]
systemctl start [service]
systemctl stop [service]
systemctl disable [service]
```

## Firewalld(类似iptables)

控制面板
```bash
# 启动
systemctl start  firewalld
# 查看状态
systemctl status firewalld 
or
firewall-cmd --state
# 停止
systemctl stop firewalld
# 禁止开机启动
systemctl disable firewalld
```
 
 
更新防火墙规则：
```bash
firewall-cmd --reload
firewall-cmd --complete-reload
#两者的区别就是第一个无需断开连接，就是firewalld特性之一动态添加规则，第二个需要断开连接，类似重启服务
```

配置
```bash
# 版本号和帮助
firewall-cmd --version
firewall-cmd --help
# 查看所有信息
firewall-cmd --list-all
```

添加服务
```bash
# 打开一个服务，类似于将端口可视化，服务需要在配置文件中添加，/etc/firewalld 目录下有services文件夹
firewall-cmd --zone=work --add-service=smtp
# 移除服务
firewall-cmd --zone=work --remove-service=smtp
```

添加接口、端口和区域
```bash
# 查看活动区域:
firewall-cmd --get-active-zones
# 设置默认接口区域,立即生效无需重启:
firewall-cmd --set-default-zone=public
# 将接口添加到区域，默认接口都在public:
firewall-cmd --zone=public --add-interface=eth0
# 永久生效再加上 --permanent 然后reload防火墙

#查看所有打开的端口：
firewall-cmd --zone=dmz --list-ports
#加入一个端口到区域,若要永久生效方法同上, port/protocol, 其中protocol可以是tcp和udp
firewall-cmd --zone=dmz --add-port=8080/tcp
firewall-cmd --add-port=2000-5000/tcp
```

还有端口转发功能、自定义复杂规则功能、lockdown，由于还没用到，以后再学习\

### SeLinux
这个服务也是默认开启的，它会让nginx代理端口转发的时候产生权限问题，从而`connect failed`，甚至无法启动。SeLinux运行模式分为三种 enforcing (强制模式)、permissive（宽容模式）、disabled（关闭）。

```bash
# 查看SeLinux状态
sestatus
# 临时关闭
setenforce 0
# 永久关闭
vi /etc/selinux/config
# 将`SELINU`置为`disabled`,然后reboot
```

很多文章会建议关闭SeLinux，但更好的方法是设置selinux策略指。允许用户通过httpd访问/var/www文件夹。可以查阅这个命令的文档，关注更多策略值。不过如果不是在/var/www文件夹下，就没有办法。
```bash
setsebool -P httpd_enables_homedirs=1 
# -P 是永久性设置，否则重启之后又恢复预设值。
```

获取本机selinux策略值，也称为bool值。
```bash
getsebool -a
```