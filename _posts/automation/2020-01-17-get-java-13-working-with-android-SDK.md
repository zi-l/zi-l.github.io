---
layout: post
title: 解决JDK 13和Android SDK不兼容的问题
subtitle: "Get Java 13 working with android SDK"
author: "Yon Liu"
tags:
  - selenium
  - automation
---

## Info
SDK    
-  [sdk-tools-darwin-4333796.zip](https://dl.google.com/android/repository/sdk-tools-darwin-4333796.zip) , 已是官方最新(
2020/1/17)

JDK     
```shell
brew install openjdk   # jdk 13
```    


## issue
执行命令`$ANDROID_HOME/tools/bin/sdkmanager --list`时出现问题
```
Exception in thread "main" java.lang.NoClassDefFoundError: javax/xml/bind/annotation/XmlSchema
```
![](/images/automation/sdkmanager-java-error.png)

参考[bug-JDK-8189188](https://bugs.openjdk.java.net/browse/JDK-8189188) 关于javax.xml.bind等模块的移除说明，Java 9仍然保留这些
模块，可以通过`--add-modules`来使用，但从Java 11起，这些模块已经被移除，无法通过`--add-modules`使用。


## solution
可以通过添加需要的包（如下）到$ANDROID_HOME/tools/lib来解决问题。参考[getting-android-sdkmanager-to-run-with-java-11](https://stackoverflow.com/questions/53076422/getting-android-sdkmanager-to-run-with-java-11) 的做法
- [javax.activation](https://search.maven.org/artifact/com.sun.activation/javax.activation)
- [jaxb-core](https://search.maven.org/artifact/com.sun.xml.bind/jaxb-core)
- [jaxb-api](https://search.maven.org/artifact/javax.xml.bind/jaxb-api)
- [jaxb-impl](https://search.maven.org/artifact/com.sun.xml.bind/jaxb-impl)

添加包后，用编辑器打开$ANDROID_HOME/tools/bin下的sdkmanager.sh，将这些包追加到CLASSPATH里（Windows为sdkmanager.bat，按windows做法添加）：
![](/images/automation/attach-to-CLASSPATH.png)

然后更新
```shell
$ANDROID_HOME/tools/bin/sdkmanager --update
```

![](/images/automation/android-repositories.cfg-could-not-be-loaded.png)
创建/var/root/.android/repositories.cfg文件和～/.android/repositories.cfg文件
```shell
touch /var/root/.android/repositories.cfg
touch ～/.android/repositories.cfg
```
安装build-tools和platform-tools
![](/images/automation/install-build-tools-platform-tools.png)

成功
![](/images/automation/adb-version.png)

*另注：要使用bin里面的工具，可能每个都需要追加CLASSPATH，如avdmanager。该问题应该可以通过java的CLASSPATH来解决*
![](/images/automation/avdmanager-CLASSPATH.png)