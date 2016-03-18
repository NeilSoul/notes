# Raspberry Pi on Mac

## usb转串口调试

1. 使用PL2303TA芯片
2. 下载驱动
```sh
# 官网
http://www.prolific.com.tw/US/ShowProduct.aspx?p_id=229&pcid=41
```
3.双击.pkg文件并重启
4.在“网络偏好设置”中，发现多了一项USB-SerialController

5.测试
```
cd /dev
ls tty.*
# 发现tty.usbserial
```

6.安装并配置minicom
```
sudo brew install minicom
```
```
sudo minicom -s
# 选择第三项Serial port setup，回车进入详细设置
# 第一项Serial Device 手动改为/dev/tty.usbserial
# Hardware Flow Control 改为NO
# Save setup as dfl
# 选择Exit，成功进入minicom

```
以后就能用minicom来调试
```
minicom
```

7.不用第三方的方法
加电。在Mac的终端下，
```
screen /dev/tty.usbserial 115200
```
如果直接拔掉USB串口板，会造成系统重启，因此需要下面的命令：
ps -x|grep tty,得到串口连接进程号
kill 进程号


8. 连接
树莓派的第一排的第三，四，五个分别是地，TX与RX，与被连接设备连接起来。注意树莓派的TX要连接PL2303TA的RX，树莓派的RX要连接PL2303TA的TX.

6,8,10分别于USB端GND, RXD, TXD相连.

本产品专门针对实验室、产品测试、低成本MCU通讯等应用，有4条引出线

红色 +5V

黑色 GND

白色 RXD

绿色 TXD


## Default Account
username:pi
password:raspberry