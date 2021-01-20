---
layout: post
title: Wireshark zigbee抓包设置
subtitle: "Wireshark settings for zigbee"
author: "Yon Liu"
tags:
  - zigbee
--- 

## 工具
- CC2531
- [TIMAC](http://www.ti.com/tool/TIMAC) (使用其中的tiwspc)

## zigbee协议设置
zigbee的Transport Key有多种，如Trust Center Link Key和Standard Network Key，zigbee抓包需要设置该两项。

#### 添加Trust Center Link Key
Trust Center Link Key在zigbee自组网中用于集中式网络的加密传输，添加Trust Center Link Key可用于抓取对应zigbee网络的Standard Network Key，默认为(ZigBeeAlliance09)：
```
5A:69:67:42:65:65:41:6C:6C:69:61:6E:63:65:30:39
```

点击 `Edit > Preferences > Protocols > Zigbee` （Mac为`Wireshark > Preferences>...` ），按照以下添加默认Trust Center Link Key
![](/images/zigbee/tcl-key.png)
#### 添加Standard Network Key
在zigbee自组网中，Standard Network Key由Coordinator随机生成，Trust Center可以周期性地更新，Trust Center使用Trust Center Link Key加密 Standard Network Key发送给入网节点。添加Standard Network Key用于解密zigbee通信，获取通信数据。
抓到Standard Network Key后，按Trust Center Link Key同样的方式添加，如：
![](/images/zigbee/transport-key.png)

## IEEE 802.15.4协议设置

#### 修改IEEE 802.15.4协议的FCS format
同样打开到Protocols，选中IEEE 802.15.4，把FCS format项改为TI CC24xx metadata
![](/images/zigbee/ieee802.15.4-fcs_format.png)