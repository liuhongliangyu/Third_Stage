# 插件下载

在使用Unity中针对HTC Vive设备进行开发，需要使用到Steam平台为提供的插件包

* AssetSotre下载:SteamVR Plugin.unitypackage.

# 插件核心类简介

这个插件中已经封装了与HTC Vive硬件进行通信的相关类，也是我们在做HTC Vive开发过程中经常使用的类，下面讲带领大家一起来了解一下。

* SteamVR_TrackedObject

此类用于根据硬件设备，并为硬件设备分配相应的索引

* SteamVR_Controller.Device

最重要的类，封装了跟踪设备的全部信息,例如手柄的各种交互相应勾动扳机等。

* SteamVR_Controller.ButtonMask

手柄各按键的名称

* SteamVR_Controller

此类通常使用静态方法Input根据设备索引值获取对应的设备(Device)对象

# 手柄按键介绍

在学习手柄操作之前，我们先来了解一下手柄上的按钮到底有那些？

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/02_HTCVive%E6%89%8B%E6%9F%84%E7%BC%96%E7%A8%8B/images/htc_shoubing.png)

每个按钮的名称都已经在ButtonMask中定义好。

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/02_HTCVive%E6%89%8B%E6%9F%84%E7%BC%96%E7%A8%8B/images/20161020180033.png)

# HTC Vive手柄基础操作

想要操作手柄的前提是先要获取到设备唯一标识也就是设备的索引,然后通过这个索引来获取的设备,也就是Device对象,然后利用Device对象的提供的相应函数进行交互。

![](https://nts.newbieol.com/static/k25/04_%E8%99%9A%E6%8B%9F%E7%8E%B0%E5%AE%9E%E5%BC%80%E5%8F%91/02_HTCVive%E6%89%8B%E6%9F%84%E7%BC%96%E7%A8%8B/images/20170311162935.jpg)

实际上在SteamVR_TrackedObject类枚举类型EIndex中，已经预先定义了16个设备索引，头盔默认索引为0.

```C#
public enum EIndex
	{
		None = -1,
		Hmd = (int)OpenVR.k_unTrackedDeviceIndex_Hmd,
		Device1,
		Device2,
		Device3,
		Device4,
		Device5,
		Device6,
		Device7,
		Device8,
		Device9,
		Device10,
		Device11,
		Device12,
		Device13,
		Device14,
		Device15
	}
```

## 【案例1】手柄扳机键按下

1.通过手柄游戏对象上的SteamVR_TrackedObject组件进行获取。

```C#
//获取跟踪对象
SteamVR_TrackedObject trackObj = this.GetComponent<SteamVR_TrackedObject>();
//输出设备Index
print(trackObj.index);

-----输出结果----
Device1
```

2.通过找到的索引获取到Device对象。

虽然第1步可以得到设备的索引，但并没有得到设备（Device），需要再做另一步操作，通过SteamVR_Controller.Input方法传入前面得到的设备索引，返回Device类型设备，之后便可以调用设备中的方法，响应手柄操作。

```C#
//通过设备索引获取到相应设备对象Device
SteamVR_Controller.Device device= SteamVR_Controller.Input((int)trackObj.index);
```

3.调用Device中相应函数，来捕获手柄的操作。

通过Device.GetPressDown / GetPressUp / GetPress/Device.GetTouchDown / GetTouchUp / GetTouch进行操作

（1）手柄按键(以Trigger键为参考，其它键不再罗列)

```C#
//按键被按下
bool triggerPressDown=device.GetPressDown(SteamVR_Controller.ButtonMask.Trigger);
if(triggerPressDown){
  print("扳机键Trigger被按下")
}
//按键被松开
if(device.GetPressUp(SteamVR_Controller.ButtonMask.Trigger)){
  print("扳机键Trigger被松开")
}

//按键被按下并保持不松开
if(device.GetPress(SteamVR_Controller.ButtonMask.Trigger)){
  print("扳机键Trigger被按下并保持")
}
```

当我们在按下扳机键的过程中会在控制台会打印以下结果

```C#
扳机键Trigger被按下并保持(多次输出)
扳机键Trigger被按下
扳机键Trigger被松开
```

其它按键扩展

在上面的案例中，只是单纯地演示了扳机键，对于其它键按键请参考SteamVR_Controller.ButtonMask中定义的按钮进行尝试。

## 触摸板

### 根据状态获取输入值

通过获取用户触摸操作，然后根据当前的状态进行获取

```C#
//获取设备
 SteamVR_Controller.Device device=SteamVR_Controller.Input((int)GetComponent<SteamVR_TrackedObject>().index);
//第一种方法
if(device.GetTouchDown(SteamVR_Controller.ButtonMask.touchpad)){
   //获取当前状态信息
  VRControllerState_t curState=device.GetState();
  //当前状态信息中的轴输入值
   print(curState.rAxis0.x+","+curState.rAxis0.y);
}
```

### 根据轴输入方式

此种方式类型于Input.GetAxis方式，传入轴，返回输Vector2类型值
获取Touchpad圆盘坐标或Trigger的行程值（0-1），函数默认参数是手柄上的Touchpad。共有5个AxisId参数可选，0是TouchPad，1是Trigger，2,3,4应该是没有用的，且此函数只接受EVRButtonId类参数而不接受ButtonMask。

```C#
private SteamVR_Controller.Device device;
void Start(){
	 device=SteamVR_Controller.Input((int)GetComponent<SteamVR_TrackedObject>().index);
}

void Update(){
	//第二种方法(比较偏底层)
	Vector2 v2=device.GetAxis(Valve.VR.EVRButtonId.k_EButton_Axis0);
	//输出轴输入值，分为x轴和y轴
	print(v2);
}
```

## 手柄的震动TriggerHapicPulse

手柄的震动通过TriggerHapicPulse方法,参数可以理解为震动的强度。
手柄震动控制函数，参数名称解释的是时间，默认500,但实际上控制的是震动的强度。默认AxisId是EVRButtonId_touchpad，选择其他EVRButtonId没用（等价参数axis0可以）,其会调用OpenVR中的同名函数。参数超过4000会无效，导致震动不触发

```C#
private SteamVR_Controller.Device device;
void Start(){
	 device=SteamVR_Controller.Input((int)GetComponent<SteamVR_TrackedObject>().index);
}

void Update(){
	//在扳机键被按下时，震动手柄
  if(device.GetPress(SteamVR_Controller.ButtonMask.Trigger)){
			device.TriggerHapticPulse(2000);
	}
}
```

## Device常用属性

* valid：GetControllerStateWithPose（）函数调用是否成功；

* connected：判断设备是否连接；

* hasTracking：判断设备是否跟踪正常；

根据ETrackingResult的结果得到下面三个参数：
  * outOfRange：判断设备是否超出范围；
  
  * calibrating：判断设备是否正在校正；
 
  * uninitialized：判断设备是否未初始化；

* transform：获取的结果是包含12个元素的一维数组，通过SteamVR_Utils.RigidTransform函数将12个元素重组为3X4矩阵并针对Unity的坐标系进行修正，同时添加了对position和rotation方便的引用。

* velocity和angularVelocity：这两个速度也针对Unity的坐标系进行修正，lighthouse跟踪的空间轴方向与Unity存在偏差。

# [案例]

通过手柄拾取物体.

```C#
using UnityEngine;
using System.Collections;

public class DragBall : MonoBehaviour {

	SteamVR_Controller.Device device;
	// Use this for initialization
	void Start () {
		//获取设备跟踪对象
		SteamVR_TrackedObject trackedObj = this.gameObject.GetComponent<SteamVR_TrackedObject> ();
		//输出设备索引
		print (trackedObj.index);
		//获取设备对象
		device=SteamVR_Controller.Input ((int)trackedObj.index);
	}


	// Update is called once per frame
	void Update () {
		if (device.GetPressDown (SteamVR_Controller.ButtonMask.Trigger)) {

			Ray ray = new Ray (this.transform.position, this.transform.forward);

			RaycastHit hitInfo;
			if (Physics.Raycast (ray, out hitInfo, 2)) {
				print (hitInfo.transform.name);
				if (hitInfo.transform.tag == "Sphere") {
					hitInfo.transform.parent = this.transform;
					hitInfo.transform.localPosition = Vector3.zero;
					hitInfo.transform.GetComponent<Rigidbody> ().useGravity = false;
				}
			}
		}

		if (device.GetPressUp (SteamVR_Controller.ButtonMask.Trigger)) {
			Transform t = this.transform.FindChild ("Sphere");
			if (t != null) {
				t.parent = null;
				t.gameObject.GetComponent<Rigidbody>().useGravity=true;
				t.gameObject.GetComponent<Rigidbody> ().velocity= device.velocity;
			}
		}
	}
}
```







