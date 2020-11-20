---
title: ubuntu因硬盘挂载进入Emergency_mode紧急模式
date: 2020-10-27 00:00:04
categories: 
- 软件配置
tags: 
- ubuntu
- 软件配置
---
## 起因
由于ubuntu系统中加的新硬盘没有配置好，fstab中没写好，然后强制关机了，导致开机没有挂载上新硬盘，于是进入了Emergency mode紧急模式，无法正常启动。
## 分析
在紧急模式下输入回车进入命令行，使用以下命令查看开机日志
```
journalctl -xb
```
发现Failed mount on XXX的红色报错信息，根据报错信息提示，发现是无法以ext3的形式挂载上新硬盘（图没拍，大家意会一下）
## 解决方法
修改fstab中的挂载信息。首先查看硬盘的UUID
```
sudo blkid
```
![blkid](https://raw.githubusercontent.com/Marshzero/PicBed/master/images/blkid.png)
### 5.编辑系统挂载配置文件/etc/fstab
```
sudo gedit etc/fstab
```
将新硬盘挂载信息从ext3改成ex4，并将/dev/sda改为UUID（一定按图示，第一项使用UUID，不要用/dev/sda这种目录形式，第二项挂载点，第三项ext4，写成ext3我就进不去系统，后面是0 2）
![addfastab](https://raw.githubusercontent.com/Marshzero/PicBed/master/images/addfastab.png)