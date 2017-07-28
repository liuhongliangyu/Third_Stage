# Photon引擎基础

## Photon介绍

### 什么是Photon

Photon是一个泛型的SocketServer套装软件，可用于偶人游戏、聊天室、大厅游戏，并同时支持Windows、Unity3D、IOS、Android、Flash等平台。

以前做网络游戏都要花费大量的金钱及人力开发游戏引擎（Game Engine）及服务器（Game Server），但是随着技术的进步及游戏开发成本越来越高，开始有了成型的游戏引擎，像Unity3D、UDK等，当引擎有了成型的，服务器当然也会出翔成型的，相比较常用的且价格便宜的服务引擎如：SmartFox Server、Electro Server、Photon Server等，当然还有收费相对较高的服务器如：BigWorld Server，这些成型的游戏服务器为游戏公司省下大量的开发费用，也让一些小型公司或独立制作团队有了开发网络游戏的能力，尤其是在社群游戏盛行的这个年代，这些成型的服务器更受欢迎。

Photon内建一套大厅游戏服务及MMO游戏服务器，都含有源代码，使用者可以拿来修改成自己所需或直接继承后加入自己的游戏逻辑中。

Photon SDK简介

简单的说，Photon就是用C#来做服务器的最佳入口

### Photon的组织架构图

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Photon%E5%9F%BA%E7%A1%80/images/PhotoServer.png)

包含下面几个特点:

* 1. 实时的,基于回合制的,或者MMO类型的

* 2. 多人处理的框架结构

* 3. 跨平台的部署

* 4. 很高的扩展性

* 5. 可定制化编程

### 平价服务器比较

市面上有几个价格较为便宜的游戏服务器，下图列出的是比较受欢迎的几个，这几个服务器都有很好的效果与架构，本课程主要是以Photon为主，其他服务器有机会建议接触一下，都有免费版本可以使用。其实不管是游戏引擎还是服务器它们都是开发工具，没必要局限于某一个上，尽量去增大自己的视野以及自己创作空间。

市面上主要的Server比较表格

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Photon%E5%9F%BA%E7%A1%80/images/Image.png)

以上是几个游戏服务器都有提供商用的免费版本,可以拿来作为商业用途,只要在前面显示Server的Logo即可.

### 使用Photon的理由

* 1. Photon的核心C++性能更优于平价服务器。

* 1. 因为Photon Server的SDK使用的C#开发，因此使用资料库比Java构架的SFS或ES5好不少，不用去就低效能的ODBC或开发门槛高的Corba。

* 1. Photon有提供Win32 C++跟Mac的客户端（Client），可选用更多的游戏，经过适当的组件包装，连不支持的平台都可以使用。

* 1. 2011年3月Photon与Unity合作提供一个MModeAsset解决方案，Unity是广受独立开发者欢迎的引擎，可用于Photon得到最佳的搭配。

* 1. Photon本身也在不断的发展，包括将来出现的Photon Clound。

### 使用Photon的问题点

* 1. Photon Server以前只能架在Windows上，官方已提出会出Linux版本（可能已经出了）

* 1. 缺少中文的文件教学，更多Photon特殊问题直接使用Photon官网 http://www.exitgames.com/

## 安装Photon

### 1. 注册会员

登录官网 http://www.exitgames.com/ 注册会员，如果要想下载想要的版本，最好是注册一个会员（免费的）

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Photon%E5%9F%BA%E7%A1%80/images/PhotoServer01.png)

### 2. 下载

在官网 http://www.exitgames.com/ 下载Photon，Client SDK可选择性下载，直接下载Server SDK就行，Server内部有内置的.Net及unity的Client SDK,不过另下载的ClientSDK的版本会更新一些。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Photon%E5%9F%BA%E7%A1%80/images/PhotoServer03.png)

Licenses 可无限制任务30天或者100人免费，因为是开发用为了省麻烦,这里我直接下载不限时间100人免费的授课[Free License](/resource/k25/03_引擎高级进阶/项目发布/Photon基础/100 CCU,no expiry)],下载到直接的硬盘中.

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Photon%E5%9F%BA%E7%A1%80/images/PhotoServer04.png)

### 3. 解压

将下载的ServerSDK解压到自己的硬盘，不需要安装，直接解压就可以，装到主机上后可将Photon设为Windows服务。

### 4. 安装授权

找到解压的目录底下的[deploy]->[bin_Win32(32位)]或者[bin_Win64(64位)]。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Photon%E5%9F%BA%E7%A1%80/images/Image2.png)

进入文件夹后，请将之前下载的License放到这个文件夹内，并将396070677@qq.com.Photon-vX.free.100-ccu.license

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Photon%E5%9F%BA%E7%A1%80/images/Image3.png)

之后只要启动Server,Photon Server 便会自己识别文件夹底下的授权文档.

### 5. 启动Server

执行[PhotonControl.exe],在工作列上会出现Photon的icon。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Photon%E5%9F%BA%E7%A1%80/images/Image4.png)

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Photon%E5%9F%BA%E7%A1%80/images/Image5.png)

接下来在Photon icon 上按右键执行[LoadBalancing(MyClound)->Start as application]之后再执行[Open Logs]

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Photon%E5%9F%BA%E7%A1%80/images/Image6.png)

等到Log画面出现[Service is running…]就是启动完成了.

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Photon%E5%9F%BA%E7%A1%80/images/Image7.png)













  
