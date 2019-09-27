---
layout: post
title: Linux自动化环境搭建
subtitle: "Linux automation environment settings"
author: "Yon Liu"
tags:
  - ubuntu
  - centos
  - automation
---
[< HOME](https://zi-l.github.io)  

## nvm
参考 [nvm-sh/nvm](https://github.com/nvm-sh/nvm#git-install)，使用git安装，注意`~/.bashrc`和`~/.profile`（centos为`~/.bash_profile`）都需要添加指定内容。
完成后，使用`source ~/.bashrc`和`. ~/.profile`更新。

## node
使用nvm管理node，如安装v10.16.3
```shell
nvm install v10.16.3
```
使用`node -v`检查是否安装成功     
创建软链接
```
sudo ln -s $HOME/.nvm/versions/node/v10.16.3/bin/node /usr/bin/node
sudo ln -s $HOME/.nvm/versions/node/v10.16.3/bin/npm /usr/bin/npm
sudo ln -s $HOME/.nvm/versions/node/v10.16.3/bin/npx /usr/bin/npx
```

## appium
参考 [appium/appium](https://github.com/appium/appium/blob/master/docs/en/about-appium/getting-started.md)，使用npm安装：
```shell
npm install -g appium
```
使用`appium -v`检查是否安装完成，然后在`/etc/profile`文件添加
```
export APPIUM_HOME=/path/to/appium/folder/
export PATH=$APPIUM_HOME:$PATH
```
如appium文件夹位于`$HOME/.nvm/versions/node/v10.16.3/lib/node_modules/appium`，设置为
```
export APPIUM_HOME=$HOME/.nvm/versions/node/v10.16.3/lib/node_modules/appium
export PATH=$APPIUM_HOME:$PATH
```
保存后，使用`source /etc/profile`更新，用`echo $APPIUM_HOME`检查

安装appium-doctor
```shell
npm install -g appium-doctor
```

## JDK
ubuntu
```shell
sudo apt-get install openjdk-8-jdk
```
centos
```shell
sudo yum install java-1.8.0-openjdk
```
安装完后，不需要设置`JAVA_HOME`


## SDK
下载sdk-tools：[developer.android](https://developer.android.google.cn/studio#downloads)   
解压到`/usr/lib/`下，如`/usr/lib/androidsdk`
```
sudo mkdir /usr/lib/androidsdk
sudo unzip sdk-tools*.zip -d /usr/lib/androidsdk/
sudo chown $USER -R /usr/lib/androidsdk
```
完成后，在`/etc/profile`文件添加
```
export ANDROID_HOME=/usr/lib/androidsdk
export PATH=$ANDROID_HOME/platform-tools:$ANDROID_HOME/tools:$PATH
```
保存后，使用`source /etc/profile`更新，用`echo $ANDROID_HOME`检查。

##### 安装build-tools和platform-tools
*注：最新的sdk-tools不再支持UI界面，需要使用命令行。*   
查看可以安装的版本
```shell
$ANDROID_HOME/tools/bin/sdkmanager --list |grep build-tools
$ANDROID_HOME/tools/bin/sdkmanager --list |grep platform-tools
```
安装，如`platform-tools`和`build-tools 29.0.2`
```
$ANDROID_HOME/tools/bin/sdkmanager "platform-tools" "build-tools;29.0.2"
```
为adb创建软链接
```
sudo ln -s $ANDROID_HOME/platform-tools/adb /usr/bin/adb
```     

## Driver
driver可以选择以下两种方式，或者保存在框架的webdriver文件夹下
- driver放在`/usr/bin/`或`/usr/local/bin`目录下
- 新建一个文件夹专门管理driver，为driver创建软链接，如$HOME/webdriver   

创建软链接
```shell
sudo ln -s $HOME/webdriver/<chromedriver-name> /usr/bin/chromedriver
sudo ln -s $HOME/webdriver/<geckodriver-name> /usr/bin/geckodriver
```



安装完重启，`/etc/profile`添加环境变量后需要重启才能完全生效。


## Troubleshooting
- [Troubleshooting on Linux](https://zi-l.github.io/2019/09/23/troubleshooting-on-linux.html)
