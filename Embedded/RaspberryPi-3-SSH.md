SSH to RaspberryPi

## Locate RaspberryPI's IP without screen

1. tool requirement : nmap [https://nmap.org/].

2. pc and debian join in one subnet.

```bash
nmap -sP 192.168.1.0/24
# or
namp -v -sP 192.168.1.1/10
```

常用的扫描类型：
1、-sP(ping的方式扫描，检查主机在线与否，不发送任何报文到目的主机,想知道目标主机是否运行，而不想进行其它扫描，这扫描方式很常用)
2、-sL（仅仅列网段内出主机的状态、端口等信息，查询端口的话用 -p port,port1......）
3、 -PS/PA/PU [portlist]（根据给定的端口用TCP或UDP报文探测：对于root用户，这个选项让nmap使用SYN包而不是ACK包来对目标主机进行扫描。如果主机正在运行就返回一个RST包(或者一个SYNACK包)）
4、-sS(TCP同步扫描(TCP SYN)：发出一个TCP同步包(SYN)，然后等待回对方应)
5、 -sF -sF -sN（秘密FIN数据包扫描、圣诞树 (Xmas Tree)、空(Null)扫描模式使用-sF、-sX或者-sN扫描显示所有的端口都是关闭的，而使用SYN扫 描显示有打开的端口，你可以确定目标主机可能运行的是Windwos系统）
6、-sU（UDP扫描：nmap首先向目标主机的每个端口发出一个0字节的UDP包，如果我们收到端口不可达的ICMP消息，端口就是关闭的，否则我们就假设它是打开的）
7、-P0 (No ping)（这个选项跳过Nmap扫描）
8、-PE/PP/PM

最后总结下常用的nmap参数
1、nmap -sP 59.69.139.0/24(扫描在线的主机)
2、nmap -sS 59.69.139-10 -p 80,22,23,52-300（SYN的扫描方式,可以加ip和端口的限制）
3、nmap -sV 59.69.139.1 -p1-65535（探测端口的服务和版本（Version） ）
4、nmap -O 192.168.1.1或者nmap  -A 192.168.1.1(探测操作系统的类型和版本)

## ssh to RaspberryPi

Optional..
SSH上去之后第一件事就是更新debian: sudo apt-get update, 升级完成后重启一下;
在SSH终端输入sudo raspi-config, 这里需要打开几个选项:

expand_rootfs – 将根分区扩展到整张SD卡;
change_pass – 默认的用户名是pi，密码是raspberry;
change_timezone – 更改时区, 选择Asia – Shanghai;
configure_keyboard, 选English（US）;
change_locale – 更改语言设置，选择en_US.UTF-8和zh_CN.UTF-8
设置完成后，选择Finish，会提示是否重启，选择Yes

## Install VNC
在树莓派上安装vnc服务端（debian）：sudo apt-get install tightvncserver
首先要修改vnc密码：SSH终端里执行vncpasswd，然后输入两遍密码。
创建vnc-server配置文件：sudo vi /etc/init.d/tightvncserver ,在这个文件里输入如下内容：

```
### BEGIN INIT INFO
# Provides:          tightvncserver
# Required-Start:    $local_fs
# Required-Stop:     $local_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start/stop tightvncserver
### END INIT INFO

# More details see:
# http://www.penguintutor.com/linux/tightvnc

### Customize this entry
# Set the USER variable to the name of the user to start tightvncserver under
export USER='pi'
### End customization required

eval cd ~$USER

case "$1" in
  start)
    su $USER -c '/usr/bin/tightvncserver -depth 16 -geometry 800x600 :1'
    echo "Starting TightVNC server for $USER "
    ;;
  stop)
    su $USER -c '/usr/bin/tightvncserver -kill :1'
    echo "Tightvncserver stopped"
    ;;
  *)
    echo "Usage: /etc/init.d/tightvncserver {start|stop}"
    exit 1
    ;;
esac
exit 0
```
然后给增加执行权限，并启动服务：
```bash
sudo chmod +x /etc/init.d/tightvncserver
sudo service tightvncserver stop
sudo service tightvncserver start
```
安装chkconfig， 并将vnc服务设为开机启动：
```bash
sudo apt-get install chkconfig
sudo chkconfig --add tightvncserver
sudo chkconfig tightvncserver on
```

## MacOS Install VNC Client

http://sourceforge.net/projects/cotvnc/
