# Ulua框架简介

[网址](http://ulua.org/index.html)

## Ulua 的发展历史

ulua一开始从AssetStore上面的一个收费开源lua热更新小插件，发展到今天被各大游戏公司争相使用的热更新技术，真的离不开各位的使用与贡献，作者一年前花钱买了ulua后，在国内第一个引用到商业游戏里面《Q灵三国》的，并且把它从一个默默无名的小插件变得如此之庞大的使用社群。ulua的组成部分是luajit(免费开源)+LuaInterface(免费开源)+luawrap.c(作者添加些函数)，然后稍微修改后就上传到AssetStore收费开源了。98%的代码都是免费开源的。

[参考网址](http://ulua.org/cstolua.html)

## ULua 框架内容介绍

* ULua 插件:   http://ulua.org/download.html

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Lua/images/42.png)

* SimpleFramework + tolua # : http://doc.ulua.org/default.asp?cateID=4

* LuaFramework:LuaFramework是基于SimpleFramework + tolua #基础上，重新构造的新框架

https://github.com/jarjin/LuaFramework_UGUI (GitHub上下载源码)

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E9%A1%B9%E7%9B%AE%E5%8F%91%E5%B8%83/Lua/images/41.png)

* ServerFramework:基于lua热更新框架的服务器框架

https://github.com/jarjin/ServerFramework

## simpleframework的安装

http://ulua.org/simpleframework.html

* 打开网页，下载SimpleFramework_UGUI_v3.7_f5（这里以siki视频案例为基础，对于初学者尽量保持一致），下载只够解压压缩包。

* 打开Unity（5.0，本人用过5.6，但是在5.6中会存在各种错误，有些方法已经被弃用了，运行不了），点击Open other,找到解压出来的文件，点击选择文件

* 在Unity的Project面板中会出现一些文件夹和文件，了解文件夹中的信息

  * Examples： 框架自带的Demo例子，如果只需要框架的同学，里面的资源可以删除掉。去“疑难解答”里面查看方法。
    
    * Builds：里面都是一些NGUI/UGUI定义的图集啊、Prefab等资源。用于生成assetbundle而准备的资源。

    * Editor：里面是例子用到的一个新手引导步骤演示的编辑器脚本。

    * Editor Default Resource：目录是新手引导步骤对话框用的的图片资源。

    * Rsources：例子里面用于演示的一个内建的GUI容器的Prefab。（可能会没有）

    * Textures：里面是Buidls目录里面图集的原图文件。
    
    * Scenes：里面一个login场景文件也是cstolua自带的性能测试场景文件。

  * Lua：框架自带的Lua源码目录，用户自定义的Lua脚本也就是放在这里面，最后打包的时候，打包脚本会将其按目录结构生成到StreaminAssets目录里面去，然后在将其上传到游戏的Web服务器上面，用于准备被每个游戏客户端下载更新他们本地的Lua脚本。达到热更目的。
  
    * 3rd：里面是第三方的一些插件lua、实例源码文件，比如：cjson、pbc、pblua、sproto等。

    * Common：公用的lua文件目录，如define.lua文件，一些变量声明，全局配置等，functions.lua常用函数库，通讯的protocal.lua协议文件。

    * Controller：控制器目录，它不依赖于某一个Lua面板，它是独立存活在Luavm中的一个操作类，操作数据、控制面板显示而已。

    * Logic：目录里面存放的是一些管理器类，比如GameManager游戏管理器、NetworkManager网络管理器，如果你有新的管理器可以放到里面。

    * View：这是面板的视图层，里面都是一些被Unity调用的面板的变量，走的是Unity GameObject的生命周期的事件调用。

    * 还有一些lua文件
  
  * Plugins：ulua底层库所在的目录，里面存放的是不同平台的底层库，之所以ulua效率高，就是它是纯c的lua虚拟机，而不是c#解释型的。

    * Andriod：安卓lua虚拟机底层库，里面分为armv7-a与Intel x86平台。

    * iOS：里面就是苹果lua虚拟机底层库。

    * ulua.bundle：里面是Mac机器的底层库。

    * x86：里面是Win32/Linux32位机器的lua虚拟机底层库。（可能不存在）

    * x86_64：里面是Win64/Linux64位机器的lua虚拟机底层库。

  * Scripts：框架的C#脚本层，之所以这个目录跟lua目录都放在最外层，为了让用户一眼都能找到，明白是什么。

    * Common：框架的公用定义类。LuaLoader（跟lua加载有关的类）、与luavm通知unity游戏对象的“LuaBehaviour”桥类。

    * ConstDefine：常量定义目录，AppConst（应用常量）ManagerName（管理器名称）NotiConst（通知常量，用于mvc消息通知）。

    * Controller：控制器目录，分为StartUpCommand启动控制器，跟常用逻辑控制器。框架接收到启动命令后，直接在启动命令里面注册所有的管理器类。

    * Framework：经过修改过的PureMVC的框架文件。

    * Manager：Unity提供基础功能的管理器类，音乐、面板、线程、资源等众多管理器。

    * Network：网络的常用辅助类，ByteBuffer字节操作封装类，网络协议类，转换器类。

    * Utility：常用工具类。

    * View：C#用的PureMVC的视图层。
    
  *   ToLua(低版本的可能是uLua)：ulua/cstolua的核心目录，里面还有经过我们修缮后ulua的基础使用例子，用户初学者最佳。

    * Core：顾名思义，ulua的核心目录，所有c#与lua的交互都是通过它进行调度的。

    * Editor：这是供cstolua去反射定义Wrap文件列表的工具类目录。

    * Examples：经过我们修改增加后ulua自带的例子。

    * Source：这个是cstolua的核心目录，里面有Base核心目录，与动态生成用于存放LuaWrap类的缓存目录。


原来版本的消息结构为PureMVC，而新版的改成了：
AppFacade + Controller层通讯（修改版）+重写的View层通讯 = 0.3.8的框架

## 热更新生成步骤简介

### 生成wrap文件

Wrap,含义包裹,在程序中有封装的意思，使用wrap相当于对一个C#文件生成了一个包裹层，让其可以在lua中调用。

* 清空wrap

在编辑器中使用 Lua –>Clear wrap files 清空了ULuaFrameWork\Cilent\Assets\LuaFramework\ToLua\Source\ 下面的wrap文件

* 生成wrap

在编辑器中使用 Lua –>Gen Lua Wrap files 生成了c#的lua文件

### 生成委托代码

生成c#的委托的lua代码，同上 使用LUa–>Gen Lua Wrap Delegates.

### 生成binder文件

生成wrap文件后，需要调用注册函数将wrap文件注册到lua中生效。

### 生成所有文件

以上步骤可以通过Lua –>Generate All 替换。

### 生成AssetBundle文件

执行菜单“LuaFramework/Build xxx Resource”，根据配置将素材文件+lua代码统一build到StreaminAssets目录下面去，生成更新时所需要使用的assetbundle。

### 配置服务器

可以使用ULua的开源服务器框架，也可以使用自定义的服务器框架，更新前需要将StreaminAssets 目录下面的内容拷贝一份到服务器上，修改客户端的url地址以及对应的工程mode 就可以完成服务器的配置。当客户端连接服务器时，就开始更新对应内容。

### 操作步骤

* 1. 点击菜单栏的按钮Lua->Clear LuaBinder File + wrap files(清空文件)

* 2. 点击菜单栏按钮Lua->Gen Lua Wrap files (生成了c#的lua文件)

* 3. 点击菜单栏按钮Game->Build Windows Resource(在Windows环境。可以选择Android环境和iphone环境)

* 4. 创建自己的热更新UGUI（根据ulua提供的案例）

## 创建自己的热更新UGUI

### 搭建UGUI界面

* 导入资源UI文件夹，将选中Sprite下的所有图片，在Inspector面板中，点击Texture Type后的选项，选择Sprite（2D and UI），这样在UGUI中就可以使用这些图片了

* 创建UI->Canvas

  * 创建Greate Empty，重命名为BottomPanel。
    
    * 创建Image，命名为ButtonSettings，选择图片Button Round Background，调试大小，位置，添加Button组件。在创建子集Image，选择图片Settings
    
    * 复制ButtonSettings对象，重命名为ButtonPeople,子集Image选择图片People
    
    * 复制ButtonSettings对象，重命名为ButtonDialog,子集Image选择图片Dialog
    
    * 将BottomPanel做成预制体，单击预制体，在Inspector面板的最下方，有Asset labels, 点击AssetBundle后的选项,选择New，输入Bottom.assetbundle,回车，那么等到我们需要打包时，就会自动将BottomPanel打包
  
  * 创建Greate Empty，重命名为SettingsPanel。

    * 创建Image，命名为Bg，选择图片Window，调整大小和位置，添加Animator组件。制作动画:选择Bg，点击菜单栏Window->Animation，生成操作面板，点击Add Project，命名SettingsPanel保存到项目值得UI->Animation文件夹下（创建新文件夹，命名为Animation），点击Add Project，选择Rect Transform->Scale:将播放栏后面的0改为60（即为60秒播放动画），将下面的Scale.x,Scale.y,Scale.z全都改为1（即为60秒Bg的Scale由0到1变化）。同理，点击SettingsPanel，选择Create New Clip，命名为SettingsPanelHide保存，同样选择60，只是将0秒时候的Scale.x,Scale.y,Scale.z全都改为1,60秒时都设为0 。设置完之后，将Bg的Scale的x,y,z都设置为0.
  
      * 创建Image，命名为Title，选择图片Title，调整位置和大小。子集：创建Text，调整位置和大小，内容为设置界面，字体选择UI文件夹中的字体（造字工房悦圆）
    
      * 创建Image，命名为ButtonClose，选择图片Button Round Background，调整大小和位置，添加Button组件。子集：创建Image，选择图片X，调整大小和位置
      
    * 将SettingsPanel做成预制体，单击预制体，在Inspector面板的最下方，有Asset labels, 点击AssetBundle后的选项,选择New，输入Settings.assetbundle,回车，那么等到我们需要打包时，就会自动将SettingsPanel打包

* 打包

  * 将UI文件夹中->Sprite文件夹下的图片全选，在Inspector面板的最下方，有Asset labels, 点击AssetBundle后的选项,选择New，输入sprite.assetbundle,回车，那么等到我们需要打包时，就会自动将图片打包
      
  * 选中UI文件夹中的造字工房悦圆字体，在Inspector面板的最下方，有Asset labels, 点击AssetBundle后的选项,选择New，输入font.assetbundle,回车，那么等到我们需要打包时，就会自动将字体打包  
      
  * 点击菜单栏按钮Lua->Gen Lua Wrap files，点击菜单栏按钮Game->Build Windows Resource。完成之后，在Project的面板上会生成一个文件夹StreamingAssets。在该文件加些会有我们打包的资源。

* 在lua中写代码（用于更新）

用LuaStudio打开项目目录下的Asserts/Lua文件夹。在LuaStudio中写代码。

  * 打开解决方案中的Lua/Login，将其中的GameManager.lua重名为GameManager.lua_backup。然后新建文件，命名为GameManager.lua
  
  ```lua
require "Common/define"
require "Controller/BottomCtrl"
require "Controller/SettingsCtrl"

GameManager = {}

local this = GameManager

function GameManager.LuaScriptPanel()
	return "Bottom", "Settings"
end 

function GameManager.OnInitOK()
	AppConst.SocketPort = 2012;
  AppConst.SocketAddress = "127.0.0.1";
  NetManager:SendConnect();
	
	BottomCtrl.Awake()
	SettingsCtrl.Awake()
end
  ```
  
  * 在lua/View文件夹下新建文件，命名为BottomPanel.lua
  
  ```Lua
BottomPanel = {}

local this = BottomPanel
local transform
local gameObject

function BottomPanel.Awake (obj)
	gameObject = obj
	transform = obj.transform
	
	this.InitPanel()
end

function BottomPanel.InitPanel()
	this.buttonSettings = transform:FindChild("ButtonSettings").gameObject
	this.buttonPeople = transform:FindChild("ButtonPeople").gameObject
	this.buttonDialog = transform:FindChild("ButtonDialog").gameObject
end
  ```
  
  * 在lua/View文件夹下新建文件，命名为SettingsPanel.lua
  
  ```lua
  
SettingsPanel = {}
local this  = SettingsPanel

local trandform 
local gameObject 

function SettingsPanel.Awake(obj)
	gameObject = obj
	trandform = obj.transform
	
	this.InitPanel()	
end

function SettingsPanel.InitPanel()
	this.aim = trandform:FindChild("Bg"):GetComponent("Animator")
	this.buttonClose = trandform:FindChild("Bg/ButtonClose").gameObject
end

  ```
  
  * 在Lua/Controller文件夹下创建新文件，命名为BottomCtrl.lua
  
  ```lua
  require "Common/define"

BottomCtrl = {}
local this = BottomCtrl
local transform
local gameObject
local bottom 

function BottomCtrl.New()
	return this
end

function BottomCtrl.Awake()
	PanelManager:CreatePanel("Bottom",this.OnCreate)
end

function BottomCtrl.OnCreate(obj)
	gameObject = obj
	transform = obj.transform
	
	bottom = gameObject:GetComponent("LuaBehaviour")
	--print(bottom)
	bottom:AddClick(BottomPanel.buttonDialog, this.OnButtonDialogClick)
	bottom:AddClick(BottomPanel.buttonPeople, this.OnButtonPeopleClick)
	bottom:AddClick(BottomPanel.buttonSettings, this.OnButtonSettingsClick)
end

function BottomCtrl.OnButtonSettingsClick()

end

function BottomCtrl.OnButtonPeopleClick()

end

function BottomCtrl.OnButtonDialogClick()

end
  ```
  
  * 在Lua/Controller文件夹下创建新文件，命名为SettingsCtrl.lua

  ```lua
  require "Common/define"

SettingsCtrl = {}

local this = SettingsCtrl

local transform
local gameObject
local settings

function SettingsCtrl.New()
	return this
end

function SettingsCtrl.Awake()
	PanelManager:CreatePanel("Settings",this.OnCreate)
end

function SettingsCtrl.OnCreate(obj)
	gameObject = obj
	transform = obj.transform
	
	settings = gameObject:GetComponent("LuaBehaviour")
	settings:AddClick(SettingsPanel.buttonClose, this.OnButtonClose)
end

function SettingsCtrl.OnButtonClose()

end
  ```
  
  * 打开lua/Common/define.lua文件，在文件中添加内容,只需在CtrlName中添加下面内容即可
  
  ```lua
	Bottom = "BottomCtrl",
	Settings = "SettingsCtrl"
  ```
  
* 然后点击菜单栏按钮Lua->Gen Lua Wrap files (生成了c#的lua文件)。 点击菜单栏按钮Game->Build Windows Resource(在Windows环境。可以选择Android环境和iphone环境)  
  
* 运行场景，即可加载到我们的资源

### 发布Android环境

* 点击菜单按钮File->Build Settins...,选择Android，点击Switch Platfrom，设置PlayerSetting..

* 点击菜单栏的按钮Lua->Clear LuaBinder File + wrap files(清空文件)，然后点击菜单栏按钮Lua->Gen Lua Wrap files (生成了c#的lua文件)。 点击菜单栏按钮Game->Build Android Resource(在Windows环境。可以选择Android环境和iphone环境)

* 修改代码，打开项目目录

  * 在SimpleFramework_UGUI-0.3.7.5\Assets\Scripts\ConstDefine中找到AppConst.cs，将本地地址修改为本地IP地址，将更新模式由默认值false改为true
  
  ```C#
  public const string WebUrl = "http://192.168.2.109:6688/";
  ```
  
  ```C#
  //将false改为true
  public const bool UpdateMode = true; 
  ```
  
  * 在\SimpleFramework_UGUI-0.3.7.5\Server\Server\Service中找到HttpServer.cs,将本地地址改为本地网址,之后保存，**生成新的解决方案**。
  
  ```C#
  host = "http://192.168.2.109:6688/";
  ```

  * 然后点击菜单栏按钮Lua->Gen Lua Wrap files (生成了c#的lua文件)。 点击菜单栏按钮Game->Build Android Resource
  
  * 最后发布Android。在SimpleFramework_UGUI-0.3.7.5\Server\Server\bin\Debug中找到SuperSocket.SocketService.exe，双击打开，在控制台上输入‘r’，即打开服务器。 在手机上安装发布的游戏，运行（注：手机要和电脑使用同一局域网），即可在手机上显示场景。之后有什么改动（更换场景UI，添加功能...）,只要重新build就行，不用再下载新的安装包。



 
