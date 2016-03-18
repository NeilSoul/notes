Optional Improved

## Configure Vim

```bash
sudo vi /etc/vim/vimrc.tiny
```

```
set nocompatible //支持方向键
set backspace=2 //支持 backsapce 前删键
```

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

