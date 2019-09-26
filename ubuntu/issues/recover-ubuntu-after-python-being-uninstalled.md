# 卸载python3后，无网络恢复ubuntu的过程
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
[![](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/list-ubuntu-archives.png)](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/list-ubuntu-archives.png)   
[![](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/list-ubuntu-archives-ubuntu-packages.png)](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/list-ubuntu-archives-ubuntu-packages.png)

该目录有所有的依赖包，使用dpkg命令安装目录下的所有`deb`包：
`sudo dpkg -i /var/cache/apt/archives/*.deb`，问题：   
[![](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/dpkg-install-deb-binary.png)](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/dpkg-install-deb-binary.png)

重启ubuntu，`reboot`后仍然进入控制台，但网卡修复，可以连接网络  
[![](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/reboot-after-deb-being-installed.png)](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/reboot-after-deb-being-installed.png)


尝试安装python3  
`sudo apt-get install python3`  
[![](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/tried-to-install-python3.png)](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/tried-to-install-python3.png)

按指示执行命令
`apt --fix-broken install`, 修复安装完，已经安装了`ubuntu-desktop`
[![](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/fix-broken-install.png)](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/fix-broken-install.png)

重启，进入ubuntu desktop：
[![](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/enter-gui.png)](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/enter-gui.png)

nvm的node软链接问题
[![](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/nvm-link-issue-with-node.png)](https://github.com/zi-l/zi-l.github.io/blob/master/images/ubuntu/issues/nvm-link-issue-with-node.png)


修复安装后有些软链接都失效了，如`gedit`：
```
bl@bl-VirtualBox:~$ whereis gedit
gedit:
```
需要重新创建软链接，或重新安装：
```
bl@bl-VirtualBox:~$ sudo apt-get install gedit
[sudo] password for bl:
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  gedit-common python3-gi-cairo
Suggested packages:
  gedit-plugins
The following NEW packages will be installed:
  gedit gedit-common python3-gi-cairo
0 upgraded, 3 newly installed, 0 to remove and 9 not upgraded.
Need to get 551 kB of archives.
After this operation, 5,036 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://cn.archive.ubuntu.com/ubuntu bionic-updates/main amd64 gedit-common all 3.28.1-1ubuntu1.2 [137 kB]
Get:2 http://cn.archive.ubuntu.com/ubuntu bionic-updates/main amd64 python3-gi-cairo amd64 3.26.1-2ubuntu1 [6,652 B]
Get:3 http://cn.archive.ubuntu.com/ubuntu bionic-updates/main amd64 gedit amd64 3.28.1-1ubuntu1.2 [408 kB]
Fetched 551 kB in 9s (60.1 kB/s)                                               
Selecting previously unselected package gedit-common.
(Reading database ... 213570 files and directories currently installed.)
Preparing to unpack .../gedit-common_3.28.1-1ubuntu1.2_all.deb ...
Unpacking gedit-common (3.28.1-1ubuntu1.2) ...
Selecting previously unselected package python3-gi-cairo.
Preparing to unpack .../python3-gi-cairo_3.26.1-2ubuntu1_amd64.deb ...
Unpacking python3-gi-cairo (3.26.1-2ubuntu1) ...
Selecting previously unselected package gedit.
Preparing to unpack .../gedit_3.28.1-1ubuntu1.2_amd64.deb ...
Unpacking gedit (3.28.1-1ubuntu1.2) ...
Processing triggers for mime-support (3.60ubuntu1) ...
Processing triggers for desktop-file-utils (0.23-1ubuntu3.18.04.2) ...
Processing triggers for libglib2.0-0:amd64 (2.56.4-0ubuntu0.18.04.4) ...
Setting up python3-gi-cairo (3.26.1-2ubuntu1) ...
Setting up gedit-common (3.28.1-1ubuntu1.2) ...
Setting up gedit (3.28.1-1ubuntu1.2) ...
update-alternatives: using /usr/bin/gedit to provide /usr/bin/gnome-text-editor (gnome-text-editor) in auto mode
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
Processing triggers for gnome-menus (3.13.3-11ubuntu1.1) ...
Processing triggers for hicolor-icon-theme (0.17-2) ...
bl@bl-VirtualBox:~$
```
修复软连接后，即修复完成，所有数据得到保留。
