---
layout: post
title: 卸载python3后，无网络恢复ubuntu的过程
subtitle: "Recover ubuntu after python being uninstalled"
author: "Yon Liu"
header-img: images/ubuntu/issue/enter-gui.png
tags:
  - ubuntu
  - adventure
---

*注：此方法不需要下载任何东西*   


### 卸载信息
- 使用的卸载命令：  
```
sudo apt purge python3
```

- 面临的问题：  
重启后进入了tty，无法进入desktop桌面，且无法上网


### 恢复ubuntu的过程

使用`locate`命令查找ubuntu相关包的位置:
```
locate ubuntu18.04
```
*以下为修复完成后的补充图*  
![](/images/ubuntu/issues/search-ubuntu-archives.png)


查找的包缓存位于`/var/cache/apt/archives`目录下，可以使用`ls`查看该目录下的包：
```
ls -la /var/cache/apt/archives
```
*以下为修复完成后的补充图*  
![](/images/ubuntu/issues/list-ubuntu-archives.png)   
![](/images/ubuntu/issues/list-ubuntu-archives-ubuntu-packages.png)

该目录有所有的依赖包，使用dpkg命令安装目录下的所有`deb`包：
`sudo dpkg -i /var/cache/apt/archives/*.deb`，问题：   
![](/images/ubuntu/issues/dpkg-install-deb-binary.png)

重启ubuntu，`reboot`后仍然进入控制台，但网卡修复，可以连接网络  
![](/images/ubuntu/issues/reboot-after-deb-being-installed.png)


尝试安装python3  
`sudo apt-get install python3`  
![](/images/ubuntu/issues/tried-to-install-python3.png)

按指示执行命令
`apt --fix-broken install`, 修复安装完，已经安装了`ubuntu-desktop`
![](/images/ubuntu/issues/fix-broken-install.png)

重启，进入ubuntu desktop：
![](/images/ubuntu/issues/enter-gui.png)

nvm的node软链接问题
![](/images/ubuntu/issues/nvm-link-issue-with-node.png)


修复安装后有些软链接都失效了，如`gedit`：
```
bl@bl-VirtualBox:~$ whereis gedit
gedit:
```
需要重新创建软链接，或重新安装：
```
sudo apt-get install gedit
```
修复软连接后，即修复完成，所有数据得到保留。
