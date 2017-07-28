# PhotonServer

## Photon Server的配置

在官网https://www.photonengine.com/en/OnPremise/Download下载服务器SDK后，解压出来四个文件夹：

* deploy：主要存放photon的服务器控制程序和服务端Demo，（注：此文件夹是重要的文件夹哦，photon server 的运行文件在次文件夹中）。打开文件夹，分别有bin_Win32和bin_Win64文件夹对应不同操作系统，打开两个文件夹后PhotonControl.exe便是photonServer的启动文件。

* doc：文档

* lib：Photon类库

* src-server：服务端Demo源代码

* build：编译的文件

### 第一步：配置服务器端

打开deploy文件夹，看到包含了不同平台编译出的Photon目录，以“bin_”为前缀命名目录，选择你的电脑对应的文件夹打开，看到PhotonControl.exe，运行后，可以在windows右下角看到一个图标，点击图标可以看到photon服务器控制菜单，这个以后再做详细介绍.

1. 打开Visual Studio，新建类库（项目），命名为ChatServer。

1. 将生成class1.cs重命名为ChatPeer.cs(用来与客户端进行通讯的类)

1. 在ChatServer下添加一个类，命名为ChatServerApplication.cs(Photon服务器部署的时候会调用)

1. 在ChatServer下的引用中添加引用，右击引用->添加引用,弹出引用管理器，点击浏览->找到解压目录中的lib文件夹，在该文件夹中找到：

* ExitGamesLibs.dll

* Photon.SocketServer.dll

* PhotonHostRuntimeInterfaces.dll

然后点击确定，就将这三个应用扩展程序添加到了引用当中。

接下来开始写代码：

1. 在ChatServer项目中打开ChatServer类，

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Photon.SocketServer;
using PhotonHostRuntimeInterfaces;

namespace ChatServer
{
    //用来与客户端进行通讯的类
    public class ChatPeer : ClientPeer   //如果是3.4版本的photon要继承PeerBase, 如果是4.0版本的Photon要继承ClientBase
    {
        //构造函数：实现父类
        public ChatPeer(InitRequest initRequest) : base(initRequest)
        {
            
        }
        //当断开连接时调用
        protected override void OnDisconnect(DisconnectReason reasonCode, string reasonDetail)
        {
            
        }
        //服务端用来处理客户端发来的消息，可以返回消息给客户端
        protected override void OnOperationRequest(OperationRequest operationRequest, SendParameters sendParameters)
        {
            
        }
    }
}

```

然后，再ChatServer项目下打开ChatServerApplication.cs类

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Photon.SocketServer;

namespace ChatServer
{
    //Photon服务器部署的时候会调用
    class ChatServerApplication : ApplicationBase
    {
        //当一个客户端连接服务端的时候调用
        protected override PeerBase CreatePeer(InitRequest initRequest)
        {
            //每当有一个客户端与服务器连接时，会创建一个peer
            ChatPeer peer = new ChatPeer(initRequest);
            return peer;
        }
        //当服务端启动时调用
        protected override void Setup()
        {
            
        }
        //当服务端停止时调用
        protected override void TearDown()
        {
            
        }
    }
}
```

完成后，在解决方案资源管理器中选中当前项目，打开属性，选择生成选项卡，点击输出路径后的浏览，找到解压后的deploy文件夹，在该文件夹下创建ChatServer文件夹，进入文件夹，创建bin文件夹，然后点击选择文件夹，确定，那么输出路径就设定好了。

然后文本编辑器打开deploy\bin_Win64\PhotonServer.config配置文件(我的是win7 64位机器，就选择这个),添加以下配置：

在<Applications Default="Master">下面添加

```C#
<Application
		    Name="ChatServer"
		    BaseDirectory="ChatServer"
		    Assembly="ChatServer"
		    Type="ChatServer.ChatServerApplication"
		    ForceAutoRestart="true"
		    WatchFiles="dll;config"
		    ExcludeFiles="log4net.config"
		    >
		  </Application>
```
* Name:项目名字

* BaseDirectory:根目录，deploy文件夹下为基础目录

* Assembly ：是在生成的类库中的bin目录下与我们项目名称相同的.dll文件的名字

* Type:是主类的全称，在这里是：MyServer.MyApplication，一定要包括命名空间

* EnableAutoRestart：是否是自动启动，表示当我们替换服务器文件时候，不用停止服务器，替换后photon会自动加载文件

* WatchFiles和ExcludeFiles这段代码放在<Default><Applications>放这里</Applications></Default>节点下面


在<TCPListeners>下面添加

```C#
<TCPListener
		    IPAddress="0.0.0.0"
		    Port="8023"
		    OverrideApplication="ChatServer"
		    PolicyFile="Policy\assets\socket-policy.xml"
		    InactivityTimeout="10000"
		  >
		  </TCPListener>
```

可以在以后的Unity连接服务器是使用本地地址和这里面设置的端口号（Port）和OverrideApplication值,如

```C#
//与服务端建立连接（TCP协议）
myPeer = new PhotonPeer(this, ConnectionProtocol.Tcp);
myPeer.Connect("127.0.0.1:8023", "ChatServer");//连接的IP，端口
```
将上面的PhotonControl.exe设置好后，保存，然后点击菜单栏的生成->生成新的解决方案，等显示输出来源显示全部生成成功后，运行托盘程序deploy\bin_Win64\PhotonControl.exe，运行它，如果托盘图标没有变灰，说明服务器运行成功。

下面开始编写客户端代码，首先从官网下载https://www.photonengine.com/Download/Photon-Unity3D-Sdk_v4-0-0-10.zip

打开unity3d编辑器，首先把Photon-Unity3D_v3-0-1-14_SDK\libs\Release\Photon3Unity3D.dll导入到Unity中，新建脚本TextPhotonClient.cs,脚本代码如下:

```C#
using UnityEngine;
using System.Collections;
using ExitGames.Client.Photon;
using System;
using System.Collections.Generic;

public class TextPhotonClient : MonoBehaviour, IPhotonPeerListener {

    //打印服务端返回的等级和消息
    public void DebugReturn(DebugLevel level, string message)
    {
        
    }
    //调用执行事件
    public void OnEvent(EventData eventData)
    {
        
    }
    //用来向服务端发送消息，并处理服务端返回的消息
    public void OnOperationResponse(OperationResponse operationResponse)
    {
        print("得到了响应");
        Dictionary<byte, object> dict = operationResponse.Parameters;   // 
        print(dict.Count);
        foreach (object param in dict.Values)  //打印字典内的值
        {
            print(param);
        }
        print(operationResponse.DebugMessage);
    }

    private bool isConnected = false;
    void OnGUI()
    {
        if (isConnected)
        {
            if (GUILayout.Button("Send a opertion To PhotonServer"))
            {
                Dictionary<byte, object> param = new Dictionary<byte, object>();
                param.Add(1, "test");
                param.Add(2, "1234");
                myPeer.OpCustom(1, param, true);  //向服务端发送请求，1代表不同的状态设定为登录
            }
        }
    }

    //检测连接状态
    public void OnStatusChanged(StatusCode statusCode)
    {
        switch (statusCode)
        {
                case StatusCode.Connect:
                    print("我已连接Photon服务器");
                    isConnected = true;
                    break;
                case StatusCode.Disconnect:
                    print("我已断开Photon服务器");
                    break;
        }
    }

    private PhotonPeer myPeer;

    // Use this for initialization
    void Start () {
        //与服务端建立连接（TCP协议）
	    myPeer = new PhotonPeer(this, ConnectionProtocol.Tcp);
        myPeer.Connect("127.0.0.1:8023", "ChatServer");//连接的IP，端口
    }
	
	// Update is called once per frame
	void Update () {
	    print("...");
        myPeer.Service();   //发送请求
	}
}
```









