[FFmpeg](http://ffmpeg.org/)是一套十分强大的开源的多媒体编解码工具。

## 安装环境

CentOS 7.0

## 首先准备编译工具

```bash
yum install -y automake autoconf libtool gcc gcc-c++ 
yum install  nasm
```

yasm
```bash
git clone git://github.com/yasm/yasm.git
cd yasm
autoreconf -fiv
./configure 
make
make install
```

## 安装视频编码库libx264

```bash
git clone git://git.videolan.org/x264.git
cd x264
./configure --enable-static --enable-shared 
make & make install
```

## 安装音频编码库libfdk-aac
```bash
git clone git://git.code.sf.net/p/opencore-amr/fdk-aac.git
cd fdk-aac
autoreconf -fiv
./configure --disable-shared
make & make install
```

## 安装ffmpeg

注意开启x264和aac选项。

```bash
./configure --enable-gpl --enable-libx264 --enable-nonfree --enable-libfdk-aac
make & make install
```


## 常见错误so lib error

在`/etc/ld.so.conf.d/`下新建一文件，如`local.conf`，其内容是 `/usr/local/lib`

然后再执行`ldconfig`