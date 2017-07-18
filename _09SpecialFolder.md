# 特殊文件夹

* Resources文件夹

* Editor文件夹

* Plugins文件夹

* StreamingAsset文件夹

* Gizmos文件夹

* Hidden Asset文件夹

## 与脚本编译顺序相关的文件夹

在Unity开发中，我们可以为自己的项目随意命名文件夹以管理游戏资源，包括代码资源。但Unity保留了一些特殊的文件夹用来作特殊用途，例如编译顺序。

Unity的脚本编译有4个阶段（phase），脚本处在哪个编译阶段取决于脚本所在的文件夹。如果你的一些脚本需要引用一些别的文件夹中定义的类，则需要关心他们的编译顺序。你引用的类需要先于你的当前类的编译。或者当你需要引用其他语言的脚本时，那么该脚本必须出在更早的编译阶段。

Unity中的4个编译阶段如下：

* 1. 处于Standard Asset，Pro Standard Asset 和 Plugins文件夹中的运行脚本（sStandard Assert文件夹需是Assets的一级子文件夹）

* 2. 处于Standard Asset，Pro Standard Asset 和 Plugins文件夹下的以Editor命名的一级子文件夹中的脚本。

* 3. 顶级Editor文件夹（即Editor是Assets的一级子文件夹）中的脚本

* 4. 其他 Editor文件夹中的脚本（例如其他文件夹下的以Editor命名的子文件夹）

另外在Assets文件夹中以WebPlayerTemplates命名的顶级文件（即Asset/WebPlayerTemplates）将不会编译。若是处于别的文件夹下的WebPlayerTemplates（如Asset/Scripts/WebPlayerTemplates）将不会防止编译。

## 特殊文件夹

### Resources文件夹

Unity允许你按需要动态加载游戏资源到场景中。Resources.Load函数可以加载项目中位于任何位置的Resources文件夹中的资源。你可以有多个Resources文件夹，不管是否是顶级文件夹都可以。

### Editor文件夹

Editor文件夹可以在根目录下，也可以在子目录里，只要名字叫Editor就可以。比如目录：/xxx/xxx/Editor是一样的，无论多少个叫Editor的文件夹都可以。Editor下面放的所有资源文件都不会被打进发布包中，并且脚本也只能在编辑时使用。一般呢，会把一些工类的脚本放在这里，或者是一些编辑时用的DLL。比如我们现在要做类似技能编辑器，那么编辑器的代码放在这里就再好不过了。因为实际运行时我们只需要编辑器生成的文件，而不需要编辑器的核心代码。

### Plugins文件夹

文件夹中存放用于扩展Unity的插件（多为C/C++写成的原生态链接库（DLLs））。这些插件可以访问第三方代码库，系统API及其他超出Unity功能的模块。

### Gizmos文件夹

Unity的Gizmos类可在Scene视口中绘制图像用来显示设计细节。Gizmos.DrawIcon函数可以在场景视口中绘制一个图标以标记特殊的对象和位置。该函数使用的图像文件需要位于Gizmos中。

### StreamingAssets文件夹

这个文件夹下的资源也会全都打包在.apk或者.ipa中，他和Resources的区别是：Resources会压缩文件，但是它不会压缩，原封不动的打包进去，并且它是一个只读文件夹，就是程序运行时只能读，不能写。他在各个平台下的路径是不同的，不过你可以用Application.streamingAssetsPath 它会根据当前的平台选择对应的路径。

有些游戏为了让所有的资源全部使用AssetBundle，会把一些初始的AssetBundle放在StreamingAssets目录下，运行程序的时候在把这些AssetBundle拷贝在Application.persistentDataPath目录下，如果这些AssetBundle又更新的话，那么下载到新的AssetBundle在把Application.persistentDataPath目录下的原有的覆盖掉。

因为Application.persistentDataPath目录是应用程序的沙盒目录，所以打包之前是没有这个目录的，直到应用程序在手机上安装完毕才有这个目录。

StreamingAssets目录下的资源都是不压缩的，所以它比较会占空间，比如你的应用装在手机上会占100M的容量，那么你又在StreamingAssets放了一个100M的AssetBundle，那么此时装在手机上就会占200M实物容量。

### Hidden Asset文件夹

在导入阶段，Unity将完全忽略以下文件夹下的资源。

* 以Hidden命名的文件夹。

* 以'.'开头的文件和文件夹

* 以'~'开头的文件和文件夹

* 以'cvs'命名的文件和文件夹

* 以'tmp'位扩展名的文件

[参考链接1](http://docs.unity3d.com/Manual/SpecialFolders.html)

[参考链接2](http://docs.unity3d.com/Manual/ScriptCompileOrderFolders.html)
