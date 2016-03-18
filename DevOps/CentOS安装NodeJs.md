## 安装环境
CentOS7.0

## 安装方式（源码编译）

安装必要编译软件和工具
```sh
$ sudp yum install gcc make gcc-c++ openssl-devel wget
wget是下载工具，其它则为编译工具
```

下载源码。从官网复制最新的版本连接
```sh
$ wget https://nodejs.org/dist/v4.2.2/node-v4.2.2.tar.gz
```

编译和安装
```sh
$ ./configure
$ make
$ sudo make install
```

检查安装是否成功
```sh
$ node --version
```

## 其它安装方式

 1. 使用已编译版本（官网提供），首先查询自己的系统，再下载已经编译好的包，解压到`/usr/local`路径即可
 2. 通过**nvm**安装
```
$ nvm install v4.2.2
```

## 使用NVM来作为版本管理

通过git来安装（顺便安装了git）
```sh
$ sudo yum -y install git
```

从官方git仓库下载NVM
```sh
$ git clone https://github.com/creationix/nvm.git ~/.nvm
放置在当前用户默认路径的.nvm文件夹下
```

设置
```sh
$ source ~/.nvm/nvm.sh
# `source ~/.nvm/nvm.sh` Add this line to ~/.bashrc, ~/.profile, or ~/.zshrc
$ source ~/.bashrc
```

测试
```sh
$ nvm list-remote
列出所有nodejs 版本
$ nvm install [version]
安装nodejs
$ nvm list
查看已安装版本
$ nvm use [version]
切换版本
$ nvm alias default [version]
```

## 卸载
源码卸载
```sh
$ make uninstall
```
或nvm管理
```sh
$ nvm uninstall [version]
```