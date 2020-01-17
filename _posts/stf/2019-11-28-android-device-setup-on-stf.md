---
layout: post
title: STF服务关于Android的设置
subtitle: "Android device setup on STF"
author: "Yon Liu"
tags:
  - STF
--- 

## adb connect/disconnect
选中需要连接的设备，`adb connect`的地址可以从`Control --> Remote debug`查看。测试完后需要使用`adb disconnect`断开设备连接，释放资源，
防止占用。
```
adb connect <ip>:<port>
adb disconnect <ip>:<port>
```


## adb key
adb key用于授权，添加后远程PC才能对设备操作。从`Settings --> Keys --> ADB Keys`添加，点击`+`添加adb key，点击`？`可以查看
添加adb key的方法。adb key位置为`~/.android/adbkey.pub`或者`C:/Users/<username>/.android/adbkey.pub`，复制里面的内容，
添加到`Key`项保存即可。



## Troubleshooting
- adb connect出现错误`failed to authenticate to xxx.xxx.xxx.xxx`：
    - 授权问题，添加adb key即可
    - `adb connect`连接多台设备时，会出现该错误，但一般几秒后会恢复     
  

## Known issues
- `adb disconnect`后仍然显示设备为`using`状态，需要手动停止。
- `adb connect`的地址，端口会有所变化，测试时需要查看是否一致。无法连接设备会出现错误`could not find a connected device`
- STF连接的远程设备，appium的录屏方法会出现问题，无法停止录屏：`Unable to stop the background screen recording...`