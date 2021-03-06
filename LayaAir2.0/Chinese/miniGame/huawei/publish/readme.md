# 华为快游戏接入与发布调试指南

> Date:  2020-08-18   LayaAirIDE-Version >=  2.8.1

## 华为快游戏发布、调试环境准备

1. 华为品牌的手机。

2. PC电脑的与手机数据连接线，并保障发布时处于连接状态。

   > 因为华为快游戏不会生成发布二维码，需要在发布的时候，就保障数据线是连通的，否则LayaAirIDE发布的时候，只会生成一个rpk的包，最终还是要联数据线，手工复制rpk包到相关路径下。

3. 安装nodejs 环境，必须要安装 10.x 稳定版本 ，如果不是的需要重新安装[node官网：https://nodejs.org/download/release/latest-v10.x/]

4. LayaAirIDE集中开发环境，LayaAir 2.8.1 或以上版本 [ 官网下载: https://ldc2.layabox.com/layadownload/?type=layaairide ]



## 华为快游戏发布与接入完整流程

### 1、发布前的准备工作检查。

为了让发布华为快游戏顺利一些，有一些检查工作我们要做。

第一、PC里，node环境、LayaAirIDE这些，都必须要安装好（ADB以及OpenSSL无需单独安装，LayaAir引擎IDE已内置）。

第二、手机里，要打开"`开发者模式`"，允许USB调试。如下图所示。

<img src="img/1.png" alt="img" style="zoom: 80%;" />  

> 打开"`开发者模式`"的操作如有疑问，也可以参考华为官方指引文档：
>
> https://developer.huawei.com/consumer/cn/doc/quickapp-open-developer-option



### 2、在LayaAirIDE里发布华为快游戏

LayaAirIDE的发布功能，内置了华为快游戏的发布功能，需要先将LayaAir引擎的项目，通过发布功能打成.rpk后缀的包。发布功能的发布平台，选择`华为快游戏`，最小平台版本当前选择1075（如有改变可以关注官方文档）。

在下图中，刷新那里，如果没连手机，显示`未发现手机，请检查设备连接`，如果连上了会识别出手机型号（华为识别出的手机型号可能与手机销售型号不符，只要能显示出来，就说明连上了）

![img](img/3.png)  

关于发布功能的使用。由于有专门的发布功能介绍文档，这里不重复介绍了。不会的可以前往官网文档查看。

链接：https://ldc2.layabox.com/doc/?nav=zh-ts-3-0-6

### 3、关于指纹证书

当项目中release签名存在时，可在发布页面上打印签名证书指纹(提交华为快游戏时会用到该指纹字符串)，

这里要注意的是，要生成release签名后，点击`打印签名证书指纹`才有效，否则会如下图所示，提示证书不存在。

![3-1](img/3-1.png) 



### 4、真机运行与测试

当LayaAirIDE发布成功后，会自动在华为手机上调起该游戏的全屏运行界面，大家可以在真机上运行测试。如果退出游戏界面，也可以从`快应用加载器`APP进入后，直接点击游戏名称二次进入。如下图所示：

![img](img/4.png)  



### 5、如何调试

华为快游戏，并没有提供快游戏环境调试工具，所以开发者需要先保障游戏的H5版本在浏览器端是没有问题的。再来调试华为快游戏。

调试华为快游戏一切靠日志，在LayaAirIDE的发布功能里，有一项是日志等级，默认的时候该选项为log等级。如下图所示。这样会包括console的log日志，以及报错日志。如果改为只是设置为error，则只显示报错日志，不会显示console日志。无论是error还是log都会在发布后，输出对应日志等级的相关输出信息，如果为off，则会不输出任何信息。

![img](img/5.png) 

关于如何查看日志，我们如果不关掉发布项目的界面，是可以直接查看输出的日志的，如下图所示：

<img src="img/2.png" alt="图" style="zoom:80%;" /> 

其实，还可以在PC命令行下，另起一个界面查看，这样，就可以不用一直开着IDE调试了。0

操作方式是在PC的命令行下输入

```
adb logcat -s jsLog
```

那发布运行后的所有日志，都会在命令行中进行显示出来。如下图所示。

<img src="img/6.png" alt="img" style="zoom: 80%;" /> 

通常情况下，日志在命令行中查看即可，如果开发者想把日志导出来，可以使用华为的快应用加载器PC助手

华为快应用PC助手使用指南参考官方地址：

https://developer.huawei.com/consumer/cn/doc/development/quickApp-Guides/quickapp-pcassistant-user-guide
