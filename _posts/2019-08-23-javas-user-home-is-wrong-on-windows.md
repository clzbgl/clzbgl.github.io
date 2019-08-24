---
layout: post
title:  "Java读取user.home环境变量"
date:   2019-08-23 22:47:42 +0800
categories: tech
typora-root-url: ..
---

## 问题

 将Java写的一个后台程序安装为Windows服务，程序会用到user.home这个变量，安装为服务后就读取不到这个变量了`System.getProperty("user.home")`

## 解决

将Java程序安装为Windows服务的是DOS批处理，在批处理中追加将当前用户主目录写到一个文件中，Java程序读取这个文件内容就可以了

```shell
@echo off
echo userhome=%USERPROFILE% > vcp.config
......略
pause
```

读取userhome变量，FileUtil用的hutool中的工具类

```java
public static String getUserHome() throws Exception {
    String userhome = null;
    File configFile = new File("vcp.config");
    if (!configFile.exists()) {
        throw new Exception("配置文件vcp.config不存在");
    }
    List<String> lines = FileUtil.readLines(configFile, "UTF-8");
    for (String tmp : lines) {
        System.out.println(tmp);
        String[] tmpArr = tmp.split("=");
        if (tmpArr.length == 2 && "userhome" .equals(tmpArr[0])) {
            userhome = tmpArr[1].trim();
        }
    }
    if (userhome == null) {
        throw new Exception("配置文件vcp.config未找到userhome配置项");
    }
    return userhome;
}
```

## 过程

一开始用的jdk7，其实在本地测试是没有问题的，安装为Window服务后读取变量就不一样了

```shell
System.getenv("USERPROFILE")得到的是这么一个值：
C:\windows\system32\config\systemprofile\
```

因为用的JDK7，搜到了这个以为是JDK7的user.home的BUG，换成JDK8问题依然没有解决

https://bugs.java.com/bugdatabase/view_bug.do?bug_id=4787931

https://bugs.openjdk.java.net/browse/JDK-8137163

https://stackoverflow.com/questions/585534/what-is-the-best-way-to-find-the-users-home-directory-in-java/586345

https://stackoverflow.com/questions/2134338/java-user-home-is-being-set-to-userprofile-and-not-being-resolved/2134775#2134775

https://stackoverflow.com/questions/16889940/on-windows-7-how-does-java-jvm-set-user-home-system-property

最后发现是Windows服务的隔离机制

[Windows 服务的Session 0 隔离机制](http://www.voidcn.com/article/p-azrjwwfc-bgr.html)

[C:\windows\system32\config\systemprofile\下创建的desktop 与服务有关](https://blog.csdn.net/viggirl/article/details/8268981)