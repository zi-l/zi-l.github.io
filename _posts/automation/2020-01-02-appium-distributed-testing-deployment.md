---
layout: post
title: appium分布式测试服务部署
subtitle: "Appium distributed testing service deployment"
author: "Yon Liu"
tags:
  - appium
  - automation
---

## Hub
appium可以将设备注册到selenium grid，使用hub来管理设备

获取镜像
```shell
sudo docker pull selenium/hub
```
创建容器
```shell
sudo docker run -d -p <port>:4444 --name <container-name> selenium/hub
```
如：
```shell
sudo docker run -d -p 4444:4444 --name selenium-hub selenium/hub
```

## Node    
appium grid设置参考: [appium.io/grid](http://appium.io/docs/cn/advanced-concepts/grid/)

#### 桌面版
desktop版本需要使用node来注册到grid
```shell
node /path/to/appium/main.js --port <port> --nodeconfig /path/to/<appium-nodeconfig>.json
```
如
```shell
node C:\Users\<username>\AppData\Local\Programs\Appium\resources\app\node_modules\appium\build\lib\main.js --port 4726 --nodeconfig D:\appiumconfig.json
```

#### 非桌面版
即npm安装的appium版本
```shell
appium --port <port> --nodeconfig /path/to/<appium-nodeconfig>.json
```

## JSON文件
JSON文件设置参考 [appium.io/grid](http://appium.io/docs/cn/advanced-concepts/grid/)：
```json
{
  "capabilities":
      [
        {
          "browserName": "<e.g._iPhone5_or_iPad4>",
          "version":"<version_of_iOS_e.g._7.1>",
          "maxInstances": 1,
          "platform":"<platform_e.g._MAC_or_ANDROID>"
        }
      ],
  "configuration":
  {
    "cleanUpCycle":2000,
    "timeout":30000,
    "proxy": "org.openqa.grid.selenium.proxy.DefaultRemoteProxy",
    "url":"http://<host_name_appium_server_or_ip-address_appium_server>:<appium_port>/wd/hub",
    "host": <host_name_appium_server_or_ip-address_appium_server>,
    "port": <appium_port>,
    "maxSession": 1,
    "register": true,
    "registerCycle": 5000,
    "hubPort": <grid_port>,
    "hubHost": "<Grid_host_name_or_grid_ip-address>"
  }
}
```
如下，capabilities为设备注册到selenium grid的信息：
```json
{
  "capabilities": [
    {
      "browserName": "Samsung Note5",
      "version": "6.0",
      "maxInstances": 1,
      "platform": "ANDROID"
    },
    {
      "browserName": "iPhone 7",
      "platformName": "iOS",
      "maxInstances": 1,
      "platform": "MAC"
    }
  ],
  "configuration": {
    "cleanUpCycle": 2000,
    "timeout": 30000,
    "proxy": "org.openqa.grid.selenium.proxy.DefaultRemoteProxy",
    "url": "http://1.2.3.4:4726/wd/hub",   ## node所在节点的地址
    "host": "1.2.3.4",   ## node所在节点的ip
    "port": 4726,
    "maxSession": 1,
    "register": true,
    "registerCycle": 5000,
    "hubPort": 4444,   ## hub所在服务器端口
    "hubHost": "1.2.3.5"   ## hub所在服务器ip
  }
}
```

## Limitation
- 除了selenium node可以通过docker托管部分浏览器，目前其他设备需要命令行注册，暂时无法daemon模式运行