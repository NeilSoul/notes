Optional Improved

## Configure Vim

```bash
sudo vi /etc/vim/vimrc.tiny
```

```
set nocompatible //支持方向键
set backspace=2 //支持 backsapce 前删键
```

## Source

The source file path is  `\etc\apt\sources.list`.

Source address recommended in china
```
http://mirrors.ustc.edu.cn/raspbian/raspbian
#中科大
http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian
#清华
```

## 屏幕分辨率

配置文件路径`/boot/config.txt`.
修改屏幕属性
```
hdmi_group=2
hdmi_mode=35
hdmi_ignore_edid=0xa5000080
```
属性列表可查，group为2是针对VGA转HDMI

## Supported IPv6
```
sudo modprobe ipv6
```

Now, enable IPv6 by default after boot:
add line after /etc/modules
```
ipv6
```
add line after /etc/network/interfaces
```
iface eth0 inet6 dhcp
```

Apt sources..
```
Asia        Singapore        NUS School of Computing SigLabs        http://mirror.nus.edu.sg/raspbian/raspbian
Asia        China        Tsinghua University Network Administrators 
```

edit `/etc/apt/sources.list`, replace the url.

```bash
sudo apt-get update
```

update easy_install
```bash
sudo easy_install -U distribute
```

