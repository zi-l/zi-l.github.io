---
layout: post
title: tshark解析zigbee包
subtitle: "Parse zigbee packets via tshark"
author: "Yon Liu"
tags:
  - zigbee
--- 

## Wireshark相关
- Wireshark：[Wireshark](https://www.wireshark.org/download.html)
- pyshark库: [github:KimiNewt/pyshark](https://github.com/KimiNewt/pyshark)（
不支持zigbee，目前只支持"wep"/"wpa-pwk"/"wpa-pwd"/"wpa-psk"这几种类型，
参考[capture](https://github.com/KimiNewt/pyshark/blob/master/src/pyshark/capture/capture.py)）


## 安装tshark

#### Mac & Windows
需要安装wireshark，tshark位于wireshark安装目录下，需添加到环境变量。默认位置为
```
Mac：/Applications/Wireshark.app/Contents/MacOS/tshark 
Win: C:\Program Files\Wireshark\tshark.exe
```

#### Ubuntu
Ubuntu有单独的tshark包（参考[launchpad:ubuntu/+source/wireshark](https://launchpad.net/ubuntu/+source/wireshark)），使用以下命令安装
```shell
sudo apt-get install tshark
```

## tshark解析zigbee包

#### tshark文档: 
- [man page](https://www.wireshark.org/docs/man-pages/tshark.html)
- [filter-reference](https://www.wireshark.org/docs/dfref/#section_z)
- [filter-man-page](https://www.wireshark.org/docs/man-pages/wireshark-filter.html)

#### zigbee抓包
从接口抓包使用-i选项，如
```shell
tshark -i \\.\pipe\tiwspc_data
```

从接口读取使用-r选项，如
```shell
tshark -r /path/to/pachket-file
```
修改配置使用-o选项，如修改fcs format和添加key
```shell
tshark -r /path/to/pachket-file -o wpan.fcs_format:"TI CC24xx metadata" \
-o 'uat:zigbee_pc_keys:"5A:69:67:42:65:65:41:6C:6C:69:61:6E:63:65:30:39","Normal",""'
```
设置过滤条件使用-Y选项，如
```shell
tshark -r /path/to/pachket-file -o wpan.fcs_format:"TI CC24xx metadata" \
-o 'uat:zigbee_pc_keys:"5A:69:67:42:65:65:41:6C:6C:69:61:6E:63:65:30:39","Normal",""' \
-Y 'frame.time >= "2020-03-09 15:14:08.590217"'
```
设置输出显示条目使用-P选项，显示明细使用-V选项
```shell
tshark -r /path/to/pachket-file -o wpan.fcs_format:"TI CC24xx metadata" \
-o 'uat:zigbee_pc_keys:"5A:69:67:42:65:65:41:6C:6C:69:61:6E:63:65:30:39","Normal",""' \
-Y 'frame.time >= "2020-03-09 15:14:08.590217"' -P -V
```
使用-T选项设置输出格式，保存为文件。如保存为json
```shell
tshark -r /path/to/pachket-file -o wpan.fcs_format:"TI CC24xx metadata" \
-o 'uat:zigbee_pc_keys:"5A:69:67:42:65:65:41:6C:6C:69:61:6E:63:65:30:39","Normal",""' \
-Y 'frame.time >= "2020-03-09 15:14:08.590217"' -T json > /path/to/saving/file.json
```