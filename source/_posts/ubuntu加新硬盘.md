---
title: ubuntu加新硬盘
date: 2020-10-26 23:58:45
categories: 
- 软件配置
tags: 
- ubuntu
- 软件配置
---
# ubuntu加装新机械硬盘并配置挂载
## 前言
因为最近要用的数据集太大了，所以加了一块2T的机械硬盘，专门用来放这个数据集。
## 准备工作
将机械硬盘装到电脑上，注意机箱的打开方式...一般硬盘放在侧面，和电源在一个面，需要电源的供电线和传输数据的SATA线。
## 挂载
### 1.开机进入ubuntu系统，使用以下命令查看硬盘情况：
```
sudo  fdisk  -l
```
![fdisk](https://raw.githubusercontent.com/Marshzero/PicBed/master/images/fdisk.png)
### 2.可以看到/dev/sda是我们的加的新硬盘，使用以下命令格式化硬盘（一定确定硬盘的目录编号！！！否则格式了系统盘就GG）：
```
sudo mkfs.ext4  /dev/sda
```
或者使用ubuntu已经安装的disk软件进行图形化界面操作（推荐用命令）
### 3.我的硬盘是直接在/media/datadisk有了挂载点，没有的话就自己创建挂载点
```
sudo mkdir /home/sda1 #示例用，后续代码不用此挂载点
```
### 4.查看硬盘的UUID
```
sudo blkid
```
![blkid](https://raw.githubusercontent.com/Marshzero/PicBed/master/images/blkid.png)
### 5.编辑系统挂载配置文件/etc/fstab
```
sudo gedit etc/fstab
```
添加新的挂载项，这样开机就挂载了（一定按图示，第一项使用UUID，不要用/dev/sda这种目录形式，第二项挂载点，第三项ext4，写成ext3我就进不去系统，后面是0 2）
![addfastab](https://raw.githubusercontent.com/Marshzero/PicBed/master/images/addfastab.png)
### 6.修改硬盘权限
其中lab是用户，777是完全权限，这一步骤是可以让非root用户对新硬盘有操作权限
```
sudo chown -R lab /media/datadisk
sudo chmod -R 777 /media/datadisk
```
