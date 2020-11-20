---
title: Ubuntu18.04双系统安装与安装后要做的事
date: 2020-10-26 23:57:15
categories: 
- 软件配置
tags: 
- ubuntu
- 软件配置
---

## Ubuntu18.04双系统安装及遇到的坑
#### 一、制作U盘安装盘
网上的教程大多是使用UltraISO(软碟通)来将iso镜像写入U盘。我的软碟通在写入时提示“设备忙”，于是尝试了网上的解决方法：关闭管家、插拔U盘、避免iso文件在U盘内存储，还是提示“设备忙”，于是转用rufus来制作安装盘，完成。  
我的笔记本主板是uefi启动，C盘128G SSD，D盘1T HDD。我把D盘分出200G的空闲空间安装ubuntu18.04，使用刻录好的U盘启动，安装时选择D盘分出的空闲200G空间。分区把200G分为192G（主分区，挂载"/"）和8G（逻辑分区，交换空间，我的电脑内存8G），这样分区适用于linux新手，可以避免ubuntu的boot分区后续空间不足的尴尬。  
安装完成后在我的bios界面就会有两个启动项：win10和ubuntu。ubuntu的启动界面也可以进入win10
> [Ubuntu U盘启动工具Rufus制作(详细步骤)
](https://www.jianshu.com/p/59d5cfe62531)

#### 二、安装好Ubuntu后要做的事
##### 1.先删几个软件
###### 删除libreoffice
```
sudo apt-get remove libreoffice-common  
```
###### 删除Amazon的链接

```
sudo apt-get remove unity-webapps-common  
```
###### 删掉基本不用的自带软件（用的时候再装也来得及）

```
sudo apt-get remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku  landscape-client-ui-install  
```

```
sudo apt-get remove onboard deja-dup  
```
> [安装Ubuntu 16.04后要做的事
](https://blog.csdn.net/qq_36498339/article/details/78642033)

##### 2.更换源
更换源这次我发现了一个新的方法，就是在软件更新中选择最佳服务器，我的是中科大的源，软件更新自己就把源更换了，很nice。  
老方法就是手动更改源：
先备份sources.list

```
cp /etc/apt/sources.list /etc/apt/sources.list.bak
```
然后将sources.list中的源更换成国内源

```
sudo gedit /etc/apt/sources.list
```
随便选以下一种源就行了

```
#阿里源
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

##中科大源
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

163源
##163源
deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse

清华源
##清华源
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```
换完了系统更新一下，第二个命令可能会很慢

```
sudo apt-get update
sudo apt-get upgrade
```

#### 三、换成N卡驱动
我的笔记本是MX150，在软件更新中点击附加驱动，选择383的专有驱动应用更改（忘了版本了），然后会让你输入一个密码，这个在后面开机时会用到，而且在开机时好像电脑的security boot要关掉。  
下载好驱动，应用更改好了，重启电脑，会出现一个蓝色背景的界面 perform mok management
正确的做法如下： 
1. 当进入蓝色背景的界面perform mok management 后，选择 enroll mok , 
2. 进入enroll mok 界面，选择 continue , 
3. 进入enroll the key 界面，选择 yes , 
4. 接下来输入你在安装驱动时输入的密码， 
5. 之后会跳到蓝色背景的界面perform mok management 选择第一个 reboot
> [配有 Nvidia 显卡的笔记本安装 ubuntu18.04 所遇到的问题与解决
](https://blog.csdn.net/bush_nj/article/details/80850937)

#### 四、美化
##### 1.安装Gnome-tweak-tool

```
sudo apt-get install gnome-tweak-tool
sudo apt-get install gnome-shell-extensions
```
安装的软件名为“优化”，打开“优化”在扩展选项卡中开启
- Dash to dock：可以对Dock栏进行自定义
- User Theme：使shell主题（即顶部菜单栏）使用桌面主题
- Blyr：使Overview有模糊背景效果
- Coverflow Alt-Tab：应用程序切换效果，类似Mac OS X
- NetSpeed：推荐，显示网速
- TopIcons Plus：推荐，后台程序显示在托盘（使deepin-wine的程序显示在托盘，必装）

##### 2.安装vimix主题和图标
Github链接：[vimix桌面主题](https://github.com/vinceliuice/vimix-gtk-themes)和[vimix图标](https://github.com/vinceliuice/vimix-icon-theme)。进入Github下载文件，主题解压到/usr/share/themes目录下，图标解压到/usr/share/icons目录下。在终端中分别执行

```
sudo ./Vimix-installer
//即执行下图所示脚本，用来安装vimix的一组主题，根据提示完成安装即可
sudo ./Installer.sh
//同上安装主题
```
> [【Linux】Ubuntu 18.04桌面美化](https://blog.csdn.net/qq_28869927/article/details/80917997)
##### 3.点击图标最小化

```
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
```

> [不美翻怎么开发!Ubuntu 16.04 LTS深度美化!(2017年度定稿版)
](https://www.jianshu.com/p/4bd2d9b1af41)

#### 五、安装常用软件

##### 1.安装shadowsocks-qt5

```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
```
加入这个源后安装会报错，因为这个源没有18.04的源，修改文件中的版本即可：

```
cd /etc/apt/sources.list.d/hzwhuang-ubuntu-ss-qt5-bionic.list
sudo vim hzwhuang-ubuntu-ss-qt5-bionic.list
```
将文件修改为

```
deb http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu xenial main
# deb-src http://ppa.launchpad.net/hzwhuang/ss-qt5/ubuntu bionic main
```
然后安装

```
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```
> [Ubuntu18.04 安装ss-qt5 缺少依赖文件](https://blog.csdn.net/a912952381/article/details/81172971)

##### 2.安装wps
在wps官网下载linux的deb安装包，下载依赖[libpng12-0](http://kr.archive.ubuntu.com/ubuntu/pool/main/libp/libpng/libpng12-0_1.2.54-1ubuntu1_amd64.deb)  
安装libpng12-0

```
sudo dpkg -i libpng12-0*.deb
```
安装wps

```
sudo dpkg -i wps*.deb
```
若出现错误，或者没有安装成功，使用如下命令修复
```
sudo apt-get install -f
```
至此，wps已经安装成功。但是由于Linux版权原因，WPS缺少字体，故我们要安装WPS所需要的字体。首先下载[WPS字体](https://pan.baidu.com/s/1QJbyRDP7yu7F3pryKgdrYQ)，然后解压。

```
sudo mkdir /usr/share/fonts/WPS-Fonts       #新建wps字体存储文件夹
cd ~/Downloads     #进入下载好的字体目录
sudo apt-get install unzip  #安装zip解压软件
sudo unzip wps_symbol_fonts.zip -d /usr/share/fonts/WPS-Fonts/  #解压字体到指定文件夹
sudo mkfontscale    #生成字体索引
sudo mkfontdir      #生成字体索引
sudo fc-cache       #更新字体缓存
```
> [Ubuntu18.04 安装后应该做的事（更新中）
](https://blog.csdn.net/hymanjack/article/details/80285400)  
> [Linux for Ubuntu 解决WPS安装缺少libpng12-0的问题](https://blog.csdn.net/tydyz/article/details/74991048)

##### 3.安装截图软件Shutter

```
sudo apt-get install shutter    #安装shutter
```
[编辑按钮变灰色](https://blog.csdn.net/hymanjack/article/details/80285400)

##### 4.安装google chrome

```
wget -q -O - https://raw.githubusercontent.com/longhr/ubuntu1604hub/master/linux_signing_key.pub | sudo apt-key add
sudo sh -c 'echo "deb [ arch=amd64 ] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
sudo apt-get update
sudo apt-get install google-chrome-stable
```

##### 5.安装搜狗输入法

```
sudo apt-get install fcitx-bin      #安装fcitx-bin
sudo apt-get update --fix-missing   #修复fcitx-bin安装失败的情况
sudo apt-get install fcitx-bin      #重新安装fcitx-bin
sudo apt-get install fcitx-table    #安装fcitx-table
```
在sougou拼音官网下载deb包

```
sudo dpkg -i sogoupinyin*.deb       #安装搜狗拼音
sudo apt-get install -f             #修复搜狗拼音安装的错误
sudo dpkg -i sogoupinyin*.deb       #重新安装搜狗拼音
```
> [Ubuntu18.04 安装后应该做的事（更新中）](https://blog.csdn.net/hymanjack/article/details/80285400)

##### 6.安装网易云音乐
在官网下载deb包，安装

```
sudo dpkg -i netease-cloud-music*.deb
```

##### 7.安装QQ、微信、迅雷、百度网盘
[Deepin wine for Ubuntu](https://github.com/wszqkzqk/deepin-wine-ubuntu)