LTE usb config.

## install usb_modeswitch
```bash
sudo apt-get install usb-modeswitch usb-modeswitch-data
```

## configure

sudo vi /etc/usb_modeswitch.conf

appending content at last: 
```
DefaultVendor= 0x12d1
DefaultProduct= 0x1f01
TargetVendor= 0x12d1
TargetProductList="14db,14dc"
MessageContent="55534243123456780000000000000011062000000101000100000000000000"
```

Well, at the time you are trying to run the manual mode-switch, the modem is already in the new mode, with a new ID:
Code:
```
Look for default devices ...
  found USB ID 12d1:14dc
   vendor ID matched
```

You need to disable the automatic mode-switching first. Edit "/etc/usb_modeswitch.conf" for that - assuming the router contains the full usb_modeswitch installation.
```
lsusb
->check HUAWEI 12d1:1f01 or 12d1:14dc
```

boot to setup
```
sudo usb_modeswitch -W -c /etc/usb_modeswitch.conf -I
```

# finally
```
sudo reboot
```


# discarded
```
EnableLogging=0
DefaultVendor= 0x12d1
DefaultProduct= 0x1f01

TargetVendor= 0x12d1
TargetProduct= 0x14dc

MessageContent="55534243123456780000000000000a11062000000000000100000000000000"
NoDriverLoading=1
```

```
/usr/share/usb_modeswitch# tar -xzvf *.tar.gz
/usr/share/usb_modeswitch# usb_modeswitch -v 12d1 -p 1f01 -c 12d1:1f01
```

## configure interface
sudo vi /etc/network/interfaces
append
```
auto eth1
allow-hotplug eth1
iface eth1 inet dhcp
```