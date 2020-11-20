---
title: ubuntu使用ss或ssr与配置全局代理
date: 2020-10-27 15:27:31
categories: 
- 软件配置
tags: 
- ubuntu
- 软件配置
---
## 前言
最近要下载一个很大的数据集，然而国内的网访问只有几十k/s，没办法只能用小飞机了。下载数据集的是一个python脚本，那只能配置全局代理了。
## 安装小飞机
### 1. 安装shadowsocks
网址：https://github.com/shadowsocks/shadowsocks  
从官网下载软件，并使用自己的账号进行配置。  
配置/etc/shadowsocks.json(没有就新建一个)

```
{
    "server":"代理服务器IP",
    "server_port":代理服务器ss服务端口,
    "password":"密码",
    "timeout":300,
    "method":"加密类型"
}
```

启动服务(本地的用sslocal，不用ssserver)

```
sudo sslocal -c /etc/shadowsocks.json
```

这时配置系统代理为127.0.0.1:8080即可科学上网
### 2. 安装shadowsocksR
网址：https://github.com/qingshuisiyuan/electron-ssr-backup  
根据以上网址安装并配置，并使用命令启动，在图形化界面配置账号信息
## 配置全局代理
Terminal只支持http、https协议，而ShadowSocks使用的是socks协议，ssr同理。我们可以使用Privoxy来将http和socks相互转换。
首先，使用下面的命令安装Privoxy：

```
sudo apt-get install privoxy
```

安装完毕后，打开Privoxy的配置文件/etc/privoxy/config:

```
sudo gedit /etc/privoxy/config
```

第一步定位到4.1. listen-address 这一段，找到监听的端口，我的是在第783行：
![IMG_4309](https://raw.githubusercontent.com/Marshzero/PicBed/master/images/IMG_4309.JPG)
可以看到端口一般都是8118。
接着找到5.2. forward-socks4, forward-socks4a, forward-socks5 and forward-socks5t这一节，加上如下配置(图中是ssr的配置):

```
forward-socks5 / 127.0.0.1:1080 .
```

![IMG_4310](https://raw.githubusercontent.com/Marshzero/PicBed/master/images/IMG_4310.JPG)
  
保存后，重启一下Privoxy:

```
sudo /etc/init.d/privoxy restart
```

接着配置终端的环境，打开终端配置文件:

```
sudo gedit ~/.bashrc
```

在末尾追加下面代码：

```
export http_proxy="127.0.0.1:8118"
export https_proxy="127.0.0.1:8118"
```

保存文件后，重启终端或者执行下面的命令重新读取配置文件：

```
source ~/.bashrc
```

使用下面代码来测试穿墙是否成功：

```
wget http://www.google.com
```

穿墙成功，接着将Privoxy添加到开机启动，在/etc/rc.local中添加如下命令，注意在exit 0之前:

```
sudo /etc/init.d/privoxy start
```

在/etc/profile的末尾添加如下两句:

```
export http_proxy="127.0.0.1:8118"
export https_proxy="127.0.0.1:8118"
``` 

到这里就可以使用Terminal来科学上网了，注意这部分是根据ss中默认本地端口为1080的情况，如果是ssr，端口是12333，要配置成相应的12333端口。其本质就是ss或ssr进行listen，系统设置代理后，出网的数据要流到127.0.0.1:1080(或12333),ss和ssr listen 后接收数据并通过自己的信道传输。加上privoxy后，就是系统代理发到privoxy，然后privoxy再发到ss ssr上，这中间privoxy可以转发sock5的，也就实现了terminal的穿墙

参考文章：
https://www.pianshen.com/article/8287306670/

