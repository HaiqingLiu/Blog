# npm与cnpm

#### 什么是npm?什么是cnpm？

**npm**(node package manager) —— node.js的包管理器，用于node插件管理（包括安装、卸载、管理依赖等）。

**cnpm**(Chinese node package manager) —— 通俗理解，就是“中国版”npm。

**说明**：因为**npm安装插件是从国外服务器下载**，受网络影响大，可能出现异常，如果npm的服务器在中国就好了，所以我们乐于分享的淘宝团队干了这事儿。来自官网：“这是一个完整npmjs.org镜像，你可以用此代替官方版本（只读），同步频率目前为十分钟一次以保证尽量与官方服务同步”。

**官方网址**：https://npm.taobao.org。

**安装**：命令提示符执行 npm install -g cnpm --registry=https//registry.npm.taobao.org  

**注意**：安装完后最好查看其版本号cnpm -v 或关闭命令提示符重新打开，安装完直接使用有可能会出现错误。

>**注**：cnpm跟npm用法完全一致，只是在执行命令时将npm改为cnpm。

>命令：  ng set --global packageManager=cnpm


【参考】

https://www.2cto.com/kf/201708/658430.html
