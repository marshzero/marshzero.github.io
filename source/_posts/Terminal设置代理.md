---
title: Terminal设置代理
date: 2020-10-27 00:00:41
categories: 
- 软件配置
tags: 
- 代理
- 软件配置
---  

## CMD设置代理
### 1. 打开cmd，在cmd中输入以下命令实现代理。
```
set http_proxy=http://127.0.0.1:1080
set https_proxy=http://127.0.0.1:1080
```
### 2. 如果你的代理服务器要求用户名和密码的话，那么还需要：
```
set http_proxy_user=
set http_proxy_pass=
 ```
### 3. 如果取消cmd代理，则将值设为空重新配置一下就好了，例如：
 ```
set http_proxy=
set https_proxy=
```
## git bash设置代理
### 1. 打开git bash，输入以下命令
```
git config --global https.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080
```
### 2. 取消代理则输入以下命令：
```
git config --global --unset http.proxy
git config --global --unset https.proxy
```
### 3. 也可以设置sock5代理
```
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'
```
### 4. 另外设置git bash代理也可以找到git的配置文件，一般在C:\Users\XXX\\.gitconfig
### 5. 当git bash使用网络命令长时间无反应或者返回403或超时等信息，检查一下代理是否设置正确，查看git配置文件或者使用以下代码查看代理：
```
git config --get http.proxy 
git config --get https.proxy
参考：https://www.cnblogs.com/cbugs/p/12257443.html
```
## npm设置代理
### 1. 设置npm代理，则在cmd或者git bash中输入以下命令
```
npm config set proxy=http://127.0.0.1:1080
npm config set https-proxy http://127.0.0.1:1080
```
### 2. 更换npm源，换成淘宝源(推荐，关于没换源装东西报错请看[这里](https://www.runoob.com))
```
npm config set registry https://registry.npm.taobao.org
```
### 3. npm取消代理
```
npm config delete proxy
npm config delete https-proxy
```
### 4. npm设置代理用户名和密码
```
npm config set proxy http://username:password@server:port
npm confit set https-proxy http://username:password@server:port
```