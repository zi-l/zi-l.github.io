---
layout: post
title: Linux自动化环境问题排除
subtitle: "Automation troubleshooting on Linux"
author: "Yon Liu"
tags:
  - ubuntu
  - centos
  - automation
--- 

#### ModuleNotFoundError: No module named 'tkinter'
tkinter依赖问题     

ubuntu  
```shell
sudo apt-get install python3-tk
```
centos  
```shell
sudo yum install python3-tkinter
```

#### src/pyodbc.h: fatal error: Python.h/sql.h: No such file or directory
pyodbc依赖问题  

ubuntu  
```shell
sudo apt-get install unixodbc unixodbc-dev
```
centos  
```shell
sudo yum install epel-release
sudo yum install python3-pip gcc-c++ python3-devel unixODBC-devel
```

##### PATH issues
- /bin/sh: 1: adb: not found
- Could not find 'aapt' in PATH. Please set the ANDROID_HOME environment variable with the Android SDK root directory path

非命令行启动时，`os.getenv`和`os.environ`无法获取`~/.bashrc`和`~/.profile`设置的环境变量。需要在`/etc/profile`添加环境变量，或者`/etc/profile.d/`添加脚本，也可以为可执行文件创建`/usr/bin`软链接
