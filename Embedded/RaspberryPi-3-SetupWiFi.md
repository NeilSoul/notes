## 无线网卡

选择无驱动无线网卡，不然需要安装驱动

## 编辑网卡配置文件

```sh
$ sudo vi /etc/network/interfaces
```

常用
```
# 头部，意义不明
auto lo
iface lo inet loopback

# 自动设置eth0
auto eth0
allow-hotplug eth0
iface eth0 inet manual

# 手动设置wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp #dhcp动态分配ip
wpa-ssid "ssid"
wpa-psk "password"
#wpa-conf [wifi-conf file path]

# 用配置文件设置wlan1
auto wlan1
allow-hotplug wlan1
iface wlan1 inet manual #手动指定ip
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

auto eth1
allow-hotplug eth1
iface eth1 inet dhcp
```


## 网卡偏好文件(*.conf)

没有密码
```
network={
	ssid="ssid"
	key_mgmt=NONE
}
```
WEP加密
```
network={
	ssid="ssid"
	key_mgmt=NONE
	wep_key0="wep password"
}
WPA/WPA2加密
```
network={
	ssid="ssid"
	key_mgmt=WPA-PSK
	psk="password"
}
```

## 重启网卡
wpa-supplicant会在保存interface几秒钟之内注意到设置已经改变，并尝试重连。
如果没有，就需要手工重启指定接口（wlan/eth0/...)
```sh
$ sudo ifdown [wlan0]
$ sudo ifup [wlan0]
```
如果不行，只能重启树莓派
```sh
# sudo reboot
```

## Install Access Point

Setup udhcpd.

```
sudo apt-get install udhcpd
```

```
sudo vi /etc/udhcpd.conf
```

conf content
```
start 192.168.1.100 #The network segment
end 192.168.1.150
interface wlan0 # The device uDHCP listens on.
remaining yes
opt dns 192.168.1.1 8.8.8.8
opt subnet 255.255.255.0
opt router 192.168.1.1
opt lease 864000
```

```
sudo vi /etc/default/udhcpd
```
comment
```
# Comment the following line to enable
#DHCPD_ENABLED="no"
```

setup wlan0:
```
sudo vi /etc/network/interfaces
```
content
```
allow-hotplug wlan0
iface wlan0 inet static
	address 192.168.1.1
	netmask 255.255.255.0
```

Setup hostapd.

```
sudo apt-get install hostapd
```

edit `/etc/hostapd/hostapd.conf`

content
```
# Basic configuration
interface=wlan0
ssid=soulpi
channel=8

# WPA and WPA2 configuration
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=3
wpa_passphrase=qwertyuiop
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP

# Hardware configuration
driver=rtl871xdrv
ieee80211n=1
hw_mode=g
devcice_name=RTL8192CU
manufacturer=Realtek
```


edit `/etc/default/hostapd`

```
DEAMON_CONF="/etc/hostapd/hostapd.conf"
```

Replace Hostapd
```
sudo apt-get remove hostapd
git clone https://github.com/jenssegers/RTL8188-hostapd.git
cd RTL8188-hostapd/hostapd
sudo make
sudo make install
```

Check that

```
sudo hostapd -dd /etc/hostapd/hostapd.conf
```

Setup NAT

```
sudo sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"
```

edit `/etc/sysctl.conf`

```
net.ipv4.ip_forward=1
```
take note of ipv6..

boot NAT
```
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT
sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
```

edit `/etc/network/interfaces`, add at last
```
up iptables-restore < /etc/iptables.ipv4.nat
```

Service..
```
sudo service hostapd start
sudo service udhcpd start
sudo update-rc.d hostapd enable
sudo update-rc.d udhcpd enable
```

Supplement about iptables
```
# list all rules
iptables -L 
# list rules on some target
iptables -L [target , eg. INPUT] --line-number
# remove regarding linenumber
iptables -D [target] [line-number]
```



