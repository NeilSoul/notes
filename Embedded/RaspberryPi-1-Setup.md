Installation Guide

## Download The Image.

Visit raspberrypi homepage [https://www.raspberrypi.org/downloads/raspbian/]. Download the latest raspberrypi img, sugest

Official images for recommended Operation System are avaliable to download from the Raspberry Pi website:[https://www.raspberrypi.org/downloads/raspbian/].

We choose `WHEEZY` in this guide book.

Unzip download zip file to [imagename].img file.


## Writing an Image to the SD card


List disk, and remember the disk identifier.
```
diskutil list
```

Unmount your SD cark by using the disk identifier to prepare copying data to it.
```
disktuil unmountDisk /dev/disk<disk# from diskutil>
# e.g. diskuitl unmoutDisk /dev/disk4
```

Copy the data to your SD card
```
sudo dd bs=1m if=[image fila path] of=/dev/rdisk<disk# from diskutil>
```

## Problem

If this command still fails, try using `disk` instead of `ridsk`, and using `1M` instead of `1m`.

4m is faster.

Using command `df -h` to check disk path.(Note: /dev/disk1s1 is one partition , /dev/disk1 is one device, /dev/rdisk1 is original device name).

## Startup and Shutdown

sudo shutdown  -h  now

