## 背景
Android 手机厂商太多，很多需要安装特定的Android usb adb驱动才能被adb识别。而Mac OS下，没有所谓的驱动。所以在研发需要真机调试的时候，会发现没法识别设备。

## 设置adb环境变量

adb是android development sdk下的工具，起调试桥的作用，全称android debug bridge。路径在`platform-tools/`下。在Mac OS 系统中，设定一下环境变量，就可以很方便得调用`adb`命令

```sh
vi ~/.bash_profile
```
写入
```
export PATH=$PATH:[SDKPATH]/platform-tools
export PATH=$PATH:[SDKPATH]/tools
```
生效
```sh
source ~/.bash_profile
```

## 查看设备的厂商ID
例如，我试用了LaunchPad里的系统信息工具
查看里面的USB信息，此时我连着我的手机，型号是魅族MX4Pro，查看到Vender ID是`0x2a45`

## 写入厂商ID
```sh
cd ~/.android
vi adb_usb.ini
```
写入上一步记住的vid，记住**最后一行不能换行**，不然重启adb会失败
```
0x2a45
```
 重启adb程序
```sh
adb kill-server
adb start-server
```

## 查看

```sh
adb devices -l
```
能看到列表中有你的正在连接的设备usb信息，则说明成功