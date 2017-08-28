# 瞬间移动插件能解决的问题

## 1.计算可导航空间

我们显然不希望玩家可以传送出边界，或者在不透明物体内部。为了解决这个问题，这个系统使用了Unity生成的导航网格作为玩家可以传送的边界。因为这个过程包括了Unity的工作，所以它很稳定并且可以放心的应用到大多数工程中。为了预加载这个数据，只需要在场景的任何位置添加一个“Vive Nav Mesh”组件，并且在检视面板点击“UpdateNavmesh Data”按钮。这样无论什么时候更新场景都可以用新的NavMesh烘焙来更新Vive Nav Mesh。上边的过程说明如下图：

## 2.选择瞬移目的地

这个系统通过简单的运动学方程使用了直观的抛物线选择机制。再次说明，这是受到了Valve的“The Lab”启发。用户将控制器举到更高的角度时，选择点会生成的更远一些。如果用户将控制器举过45度（抛物线的最大距离），角度将会锁定在那个距离。

## 3.表现游戏区域

很有必要知道传送后防护边界在哪里。因此系统围绕防护边界划定了一个盒子。

## 4.减少不适感

瞬移（显示为“眨眼”）时屏幕的淡入淡出可降低用户的疲劳和眩晕感。

# 插件的下载

* GitHub地址:https://github.com/Flafla2/Vive-Teleporter

# 插件的简介

插件里提供了两个简单场景：一个直接集成了SteamVR，另一个适用于没有HTC Vive的小伙伴来演示系统功能。资源代码已经做了文档和注释，或者可以跟随MIT Licence（参照LICENSE.txt）使用.

下图是插件作者给出的系演示效果：

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/05-VR%E7%9E%AC%E9%97%B4%E7%A7%BB%E5%8A%A8/images/htc_vive_telport_movesystem.gif)

在整个制作过程中，需要使用到三个组件：

* Vive Nav Mesh组件
 控制Unity的NavMesh系统到可渲染网格的转换。它还会计算NavMesh的边界，所以当玩家选择传送位置时可以被显示出来。

* Parabolic Pointer组件
 生成/显示一个指示网格并从Vive Nav Mesh进行采样。

* Vive Teleporter组件
控制实际传送机制。它从Parabolic Pointer找出指示数据这样就知道要传送到哪里。当用户决定传送时它还会平稳的淡入淡出屏幕防止带来的不适感。它还可以和SteamVR配合来控制按钮点击事件、控制器管理、触觉反馈，当选择传送位置后还可以显示传送区域边界。

注：Teleport Vive和Parabolic Pointer组件都会自动添加一个Border Renderer组件。Border Renderer仅仅生成并渲染出显示 ViveNav Mesh边界的网格和SteamVR游戏区域。

# 瞬间移动插件的使用方法

## 1. 配置Vive NavMesh

从添加Vive Nav Mesh对象开始，可以在Assets文件夹中的Vive-Teleporter/Prefabs/Navmesh.prefab路径下找到一个预配置的Vive NavMesh。可以将这个对象放在场景层级面板的任何地方和场景中的任何位置。
接下来需要在Unity中烘焙一个导航网格(“Navmesh”)。这个可以在Navigation窗口中完成(Window > Navigation)。

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/05-VR%E7%9E%AC%E9%97%B4%E7%A7%BB%E5%8A%A8/images/20161021130742.png)

这里有几点需要考虑:

* 系统自动剔除斜坡导航网格三角形。这意味着任何没有直接面向上的部分的导航网格都会被传送系统忽略。这个在VR中是合理的，因为玩家不能走上斜坡！

* 必须在所以可传送表面使用物理碰撞器。抛物线的点（见下边第二步）使用物理射线来确定玩家指向。因此所有可传送表面必须有碰撞器（包括像墙这种不可传送的表面并且要阻止指示）。

* 为不可传送区域分配不同的导航区域也是个不错的主意。这个对于优化（因此当玩家选择传送位置时系统不需要渲染巨大的预览网格）和游戏平衡（这样玩家就不会传送到地图以外了）很有帮助。

烘焙完导航网格之后（使用Navigation窗口底部的“Bake”按钮），回到之前创建的Vive Nav Mesh 对象。如果决定专用的导航区域（见上方），可以通过Area Mask属性选择那些区域是可传送的。然后点击检视面板中的“Update Navmesh Data”按钮，就会看到导航网格显示在场景视图中。

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/05-VR%E7%9E%AC%E9%97%B4%E7%A7%BB%E5%8A%A8/images/20161021130950.png)

## 2. 配置ParabolicPointer

接下来添加Parabolic Pointer 对象。可以在Assets文件夹中 Vive-Teleporter/Prefabs/Pointer.prefab路径下找到一个预配置的指针。可以将它放在场景层级面板的任何地方和场景中的任何位置。
你当然可以修改Parabolic Pointer脚本中的任何设置了，不过只允许设置其中的一个：配置从第一步的ViveNav Mesh对象到指针的“Nav Mesh”。

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/05-VR%E7%9E%AC%E9%97%B4%E7%A7%BB%E5%8A%A8/images/20161021131110.png)

## 3. 配置Vive Teleporter

最后需要为SteamVR Camera添加一个Vive Teleporter （Component > Vive Teleporter > Vive Teleporter）组件。这是用来渲染Vive显示的摄像机。如果是使用了SteamVR 插件中的[CameraRig]预制件则应该将Vive Teleporter 添加给那个预制件中的Camera (eye) 对象。

接下来为组件属性配置以下值：

* Pointer：将这个设置为第二步创建的Parabolic Pointer 对象。

* OriginTransform：将这个设置为追踪空间的起点。如果使用了SteamVR插件，这个就是[CameraRig] 游戏对象。当玩家传送时这个对象是实际移动的。

* HeadTransform：将这个设置为玩家的头部。这个应该是Origin Transform的子集。如果使用了SteamVR插件，这个是Camera (head) 游戏对象。

* Navmesh Animator：将这个设置为第一步创建的Vive Nav Mesh 对象的动画。

* Fade Material：将这个设置为Vive-Teleporter/Art/Materials/FadeBlack.mat中的材质

* Controllers：将SteamVR控制器对象填到这里。如果使用了SteamVR的 [CameraRig] 预制件，则应该将Controller(left) 和 Controller(right) 两个对象填到这里。

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/05-VR%E7%9E%AC%E9%97%B4%E7%A7%BB%E5%8A%A8/images/20161021131145.png)

## 4. 第四步 运行测试

运行后，通过按下手柄上的TouchPad进行瞬移操作。

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/05-VR%E7%9E%AC%E9%97%B4%E7%A7%BB%E5%8A%A8/images/20161021131426.png)



