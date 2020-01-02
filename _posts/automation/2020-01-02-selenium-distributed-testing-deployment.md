---
layout: post
title: selenium分布式测试部署
subtitle: "selenium distributed testing deployment"
author: "Yon Liu"
tags:
  - selenium
  - automation
---

## Hub
#### docker
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
#### 非docker
下载selenium grid: [selenium/downloads](https://selenium.dev/downloads/)  

命令行通过选项-role hub启动hub:
```shell
java -jar selenium-server-standalone-<xxxx>.jar -role hub
```
hub服务默认端口为4444，console地址为 `http://localhost:4444/grid/console`  

可以通过-hubConfig选项来配置hub：
```shell
java -jar selenium-server-standalone.jar -role hub -hubConfig hubConfig.json
```
参考[configuring-the-hub-by-json](https://github.com/SeleniumHQ/selenium/wiki/Grid2#configuring-the-hub-by-json)，如
```json
{
   "browserTimeout" : 30,
   "debug": false,
   "port": 4444,
   "timeout": 600,
   "cleanUpCycle": 3000
}
```


## Node  
#### docker
目前docker仅能托管chrome和firefox，官方库：[SeleniumHQ/docker-selenium](https://github.com/SeleniumHQ/docker-selenium)  

***chrome***    
获取selenium/node-chrome 或者selenium/node-chrome-debug 镜像
```shell
sudo docker pull selenium/node-chrome-debug     
## sudo docker pull selenium/node-chrome
```
创建容器，注册到hub
```shell
sudo docker run -d -p <port>:5555 -e HUB_HOST=<hub-ip> -e HUB_PORT=<hub-port> -e REMOTE_HOST="http://<node-ip>:<node-port>" --name <container-name> selenium/node-chrome-debug
```

***firefox***   
获取selenium/node-firefox 或者selenium/node-firefox-debug 镜像
```shell
sudo docker pull selenium/node-chrome-debug     
## sudo docker pull selenium/node-chrome
```
创建容器，注册到hub
```shell
sudo docker run -d -p <port>:5555 -e HUB_HOST=<hub-ip> -e HUB_PORT=<hub-port> -e REMOTE_HOST="http://<node-ip>:<node-port>" --name <container-name> selenium/node-firefox-debug
```


#### 非docker  
命令行通过选项-role node启动node，并注册到hub
```shell
java -jar selenium-server-standalone.jar -role node -hub http://<localhost-or-ip>:<hub-port>/grid/register
```

通过选项-nodeConfig配置设备信息：
```shell
java -jar selenium-server-standalone.jar -role node -nodeConfig nodeconfig.json
```
json设置参考[configuring-the-nodes-by-json](https://github.com/SeleniumHQ/selenium/wiki/Grid2#configuring-the-nodes-by-json)，如
```
{
   "capabilities":
      [
         {
            "browserName": "firefox",
            "maxInstances": 2
         },
         {
            "browserName": "chrome",
            "maxInstances": 2
         }
      ],
   "hub": "http://<hub-ip>:<port>/grid/register",
   "timeout": 600
}
```

## Limitation
- 除了docker node可以托管的浏览器，目前其他设备需要命令行注册，暂时无法daemon模式运行