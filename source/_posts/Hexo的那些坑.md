---
title: Hexo的那些坑
tags: Hexo
abbrlink: 786487f2
date: 2020-03-14 08:08:54
---
# Hexo搭建个人博客的步骤
傻瓜式的操作，一分钟学会
<!--more-->
**0. 安装Git Node.js npm**
```bash
sudo apt-get install git nodejs npm 
```
因为npm 官方默认源在墙外所以会出现没进度的情况，所以需要更换国内的淘宝源：

```bash
npm config set registry http://registry.npm.taobao.org
```
## 1. 安装Hexo
Git Node.js npm 这些都安装完成后就可以安装今天的主角Hexo了
```bash
npm install -g hexo-cli
```
用hexo -v 查看一下版本确保已经安装就绪。
接下来初始化一下hexo
```bash
hexo init filename
```
 filename是你赋予hexo安装的文件夹
成功完成hexo的初始化应该是没有报错的，就像这样：
![成功初始化Hexo](https://s1.ax1x.com/2020/03/14/8MyXfe.md.png)
初始化后的目录是这样：
![Hexo目录](https://s1.ax1x.com/2020/03/14/8MyOYD.png)
如果是直接安装到当前目录下就可以不加filename参数
当然，当前文件夹是需要没有任何文件的，隐藏的文件或文件夹也不行，如果有就会报错：
![报错目录非空](https://s1.ax1x.com/2020/03/14/8Myz6A.png)
然后就是生成我们的博客文件：
`hexo g` 
![生成博客](https://s1.ax1x.com/2020/03/14/8Myxld.md.png)
最后一步，部署到hexo本地服务器了：
```bash
hexo s
``` 
![部署到hexo本地服务器](https://s1.ax1x.com/2020/03/14/8M6PTf.png)

*将hexo部署到GitHub*

*设置个人域名*
*发布文章*

