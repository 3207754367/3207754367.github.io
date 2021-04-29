---
title: 自建frp内网穿透
categories: linux
comments: true
thumbnail: 'https://gitee.com/mpcloud/mypicture/raw/master/86b28955.jpg'
description: 在vps上自建属于自己的内网穿透服务
mathjax: true
abbrlink: 3db97f61
---
<!--more-->
### 使用frp在vps上搭建属于自己的内网穿透服务
- **服务端**
```
wget https://github.com/fatedier/frp/releases/download/v0.36.2/frp_0.36.2_linux_amd64.tar.gz 
tar -zxvf frp*.tar.gz && cd frp_0.36.2_linux_amd64
```
- 编辑frps.ini 写入服务端配置文件
``` ini
[common]
bind_addr = 0.0.0.0    #服务端IP
bind_port = 7000       #服务端端口
kcp_bind_port = 7000       #服务端kcp协议端口
dashboard_addr = 0.0.0.0   #在线仪表盘IP
dashboard_port = 8888      #仪表盘端口
dashboard_user = admin     #仪表盘账号
dashboard_pwd = 123456     #仪表盘密码
token = 123456              #连接密钥
privilege_mode = true      #启用特权模式
privilege_token = 12345678 #特权模式密码
allow_ports = 4000-50000
#限定客户端可穿透的端口 删掉此项则无限制
authentication_timeout = 0  #frps不对客户端时间戳进行超时校验
custom_404_page = https://love.1919810.com/404.html #自定义404页面
```
### 防火墙开放端口
```
firewall-cmd --permanent --add-port=22/tcp --add-port=80/tcp  #允许22 80端口通过防火墙
firewall-cmd --permanent --add-port=4000-50000/tcp  #设定允许的端口池
firewall-cmd --reload  #刷新防火墙规则
```
### 使用systemctl 工具让服务端开机自启动
- 编辑当前 systemd目录下的frs.service 配置文件
```
[Unit]
Description=Frp Server Service
After=network.target

[Service]
Type=simple
ExecStart= frps可执行文件绝对路径 -c  frps.ini配置文件绝对路径

[Install]
WantedBy=multi-user.target
```
- 加载frps.service文件
```

sudo systemctl  enable  `pwd`/systemd/frps.service
sudo systemctl  daemon-reload
sudo systemctl  restart frps.service
```
<img src="https://gitee.com/mpcloud/my_picture_bed/raw/master/c3aac432.jpg" width="1080" alt="效果图"></img>

- **客户端**
```
wget https://github.com/fatedier/frp/releases/download/v0.36.2/frp_0.36.2_linux_arm64.tar.gz 
tar -zxvf frp*.tar.gz && cd frp_0.36.2_linux_arm64
```
- 编辑客户端配置文件frpc.ini
```
[common]
server_addr =   #服务端IP或域名
server_port = 7000  #服务端监听端口
token =  123456  #服务端链接密钥
privilege_token = 12345678 #特权模式密码
dns_server = 8.8.8.8 #指定dns解析地址

[web]           #穿透服务名
privilege_mode = true #启用特权模式
local_ip = 127.0.0.1  #本地IP
local_port = 8080   #需穿透的本地服务端口
type = http        #穿透类型
remote_port = 6666  #设定远端使用的端口
subdomain =  #web域名
use_encryption = true  #启用加密
use_compression = true #启用压缩
```
- 启动穿透隧道 
./frpc -c frpc.ini
<img src="https://gitee.com/mpcloud/my_picture_bed/raw/master/86b28955.jpg" width="1080" alt="效果"></img>
