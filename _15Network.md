# Network编程基础

## Networ介绍

UnityEngine.Network是实现网络功能的核心之一，提供了基本的功能函数，例如建立服务器和加入服务器等。

## 函数介绍

```C#
Network.InitializeServer(int connections, int listenPort, bool useNat);
```

该函数适用于初始化服务器，返回NetworkConnectionError类型，如果返回NetworkConnectionError.NoError,表示创建服务器成功，connections是允许入站或玩家的数量，listPort是要监听的端口，UseNat是设置Nat穿透功能。如果你想要这个服务器能够接受链接使用Nat穿透，使用facilitator，设置这个为true。

```C#
Network.Connect(string iP, int Port);
```

该函数用于连接服务器，IP是所要连接的服务器的IP地址，Port是端口号。返回枚举类型NetworkConnectionError。如果返回NetworkConnectionError.NoError是表示连接服务器成功。

```C#
Network.Disconnect();
```

该函数用于断开网络连接。如果是服务器的话，则是断开网络连接并连接服务器。

【练习】 创建服务器，链接服务器，客户端可以与服务器进行交互

搭建UI：创建2DUI

* 创建Button，命名为btn_Server,将其子集Text重命名为Server_text,表示创建服务器，在子集Text中输入“创建服务器”

* 创建Buuton，命名为btn_Client, 将其子集Text重命名为Client_text，表示连接服务器，在子集Text中输入“连接服务器”

* 创建Text， 重命名为Client_Count， 表示连接服务器的玩家数量

* 创建Panel
  
  * 创建InputField，重命名为InputFieldChat, 表示聊天输入框
  
  * 创建Text，重命名为ChatText，表示聊天内容显示框
  
  * 创建Button，重命名为SendBtn，表示消息发送按钮，在子集Text中输入“发送”

 代码如下：

```C#
using UnityEngine;
using System.Collections;
using UnityEngine.UI;

public class NetWorkTest : MonoBehaviour
{

    private string Ip = "127.0.0.1";  //服务器地址，本地地址
    private int Port = 1000;   //端口号
    //private string Ip = "192.168.2.118";  //服务器地址，其他人的地址
    //private int Port = 9091;   //别人的端口号
    private Button btn_Server;
    private Text Server_text;
    private bool isServer;

    private Button btn_Client;
    private bool isClient;
    private Text Client_text;

    private Text Client_Count;

    private NetworkView view;
    private InputField inputfieldChat;
    private Button sendBtn;
    private Text chatText;

    void Awake()
    {
        view = this.GetComponent<NetworkView>();
    }
    // Use this for initialization
    void Start ()
    {
        btn_Server = transform.Find("btn_Server").GetComponent<Button>();
        btn_Server.onClick.AddListener(InitServer);
        Server_text = transform.Find("btn_Server/Server_text").GetComponent<Text>();
        btn_Client = transform.Find("btn_Client").GetComponent<Button>();
        btn_Client.onClick.AddListener(InitClient);
        Client_text = transform.Find("btn_Client/Client_text").GetComponent<Text>();
        Client_Count = transform.Find("Client_Count").GetComponent<Text>();

        inputfieldChat = transform.Find("Panel/InputFieldChat").GetComponent<InputField>();
        sendBtn = transform.Find("Panel/SendBtn").GetComponent<Button>();
        sendBtn.onClick.AddListener(SendMsg);
        chatText = transform.Find("Panel/ChatText").GetComponent<Text>();
    }

    //服务端：创建与断开服务器
    void InitServer()
    {
        isServer = !isServer;
        if (isServer)
        {
            //Network.InitializeServer（客户端个数， 端口号， 是否Nat穿越）返回NetworkConnectionError类型  
            NetworkConnectionError error = Network.InitializeServer(10, Port, false);
            if (error == NetworkConnectionError.NoError) //
            {
                print("服务器创建成功！");
                Server_text.text = "断开服务器";
            }
        }
        else
        {
            Network.Disconnect();
            print("服务器断开成功！");
            Server_text.text = "创建服务器";
        }
    }

    //客户端：连接与断开服务器
    void InitClient()
    {
        isClient = !isClient;
        if (isClient)
        {
            //                                                                  Ip地址，端口号
            NetworkConnectionError error = Network.Connect(Ip, Port);
            if (error == NetworkConnectionError.NoError)
            {
                print("已连接服务器");
                Client_text.text = "与服务器断开";
            }
        }
        else
        {
            Network.Disconnect();
            print("已断开服务器");
            Client_text.text = "与服务器连接";
        }
    }

    //当成功连接到服务器时，在客户端调用这个函数
    void OnConnectedToServer()
    {
        print("成功连接到服务器");
    }

    //在服务器上当连接已经断开，在客户端调用这个函数，但当连接已经被禁用，也在服务器上调用
    void OnDisconnectedFromServer(NetworkDisconnection info)
    {
        print(111);
    }
    //当一个新玩家成功连接时，在服务器上调用这个函数
    void OnPlayerConnected(NetworkPlayer player)
    {
        print("连接的客户端Ip" + player.ipAddress + " 和端口号" + player.port);
        Client_Count.text = "当前链接数量：" + Network.connections.Length;
    }

    //当一个玩家从服务器上断开时，在服务器上调用这个函数
    void OnPlayerDisconnected(NetworkPlayer player)
    {
        print("断开的客户端Ip" + player.ipAddress + " 和端口号" + player.port);
    }

    // Update is called once per frame
    void Update ()
    {
        Client_Count.text = "当前链接数量：" + Network.connections.Length;
        chatText.text = textRecoud;
    }

    void SendMsg()
    {
        view.RPC("Send", RPCMode.All, inputfieldChat.text);
        inputfieldChat.text = "";
        
    }

    private string textRecoud;
    [RPC]
    void Send(string text)
    {
        textRecoud += text+"\n";
    }
}
```

将创建好的场景发布PC，运行Unity场景，点击创建服务器按钮，打开发布的软件，点击链接服务器，那么服务端与客户端就可以进行交流。

在Unity中，如果将脚本的IP地址和端口号改为其他人的IP和端口号，那么当连接服务器时，就会与服务端进行交流。

# NetworkView介绍

NetworkView是Unity封装的一套快速实现多人联机游戏的功能。以此为基础，我们可以开发各种类型的多人游戏，可以开发过关游戏的双人联机，也可以开发类似于CS的设计游戏，以房间为单位。

## 基本介绍

* 1. 属性

```C#
isMine :NetworkView 上是否由本机创建的
observed :指定被观察及同步的组件或脚本。
stateSynchronization ：NetworkView的类型。OFF为没有数据将被同步，ReliableDeltaCompressed为当前数据发生变化时才发生，Unreiable为强制发送
viewID ：NetworkViewID类型的ID
```

* 2. 函数

```C#
RPC(Remote Procedure Call, 远程过程调用)。在Network系统中可以理解为向其他机器发送消息，如果有通过函数将被调用。
networkView.RPC(string funName, RPCmode, params object[] args);
参数funName为函数名，参数mode指定发送的范围，可选范围包括全部链接的机器包括本机，例如，All是给所有机器，Server是发送给服务器。
args是传递的参数，可传可不传，支持类型包括int，float，string，NetworkPlayer，NetworkViewID，Vector3，Quaternion.
只要有NetworkView，那么任由游戏对象上的脚本的函数体前用[RPC]标注，就可以通过RPC来通过调用，但是函数名必须是唯一的，否则其中只有一个会被调用。
```

## 语法

```C#
[RPC]
void funName()
{
  ...
}
```

【示例】简易聊天室

首先创建UI如下所示：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/NetWorkView/images/Image.png)
![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/NetWorkView/images/Image1.png)
![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/NetWorkView/images/Image2.png)

添加代码

```C#
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics;
using System.IO;
using UnityEngine.UI;

public class NetworkViewChatTest : UIHandler
{

    private string IP = "127.0.0.1";   //创建或链接的服务器地址
    private int Port = 1000;    //端口号

    private string myName;
    private string textRecord = "";  //聊天消息记录
    private string textToSend = "";   //将要发送的消息

    private Dictionary<string, string> manlist = new Dictionary<string, string>();
    private List<string> names = new List<string>();  //聊天室当前人员列表
    private bool isChat;   //控制是否可以聊天

    private NetworkView networkView;     //NetworkView组件

    private GameObject obj;   //通过obj获取组件
    private Button CreateServer_btn, ClientConnect_btn, SendButton, DisconnectBtn;
    private Text CurClientNum, ChatTalk, NameChat, DisconnectText;
    private InputField NameInputField,ChatInputField;
    private Image ChatPanel, CreatePanel;
    //string txt = "当前房间人员：\n";   //实验没成功
    void Awake()
    {
        networkView = GetComponent<NetworkView>();   //获取组件
    }
   // Use this for initialization
    void Start ()
    {
        obj = GetByName("CreateServer");
        if (obj != null)
        {
            CreateServer_btn = obj.GetComponent<Button>();
            CreateServer_btn.onClick.AddListener(CreateServer);
        }
        obj = GetByName("ClientConnect");
        if (obj != null)
        {
            ClientConnect_btn = obj.GetComponent<Button>();
            ClientConnect_btn.onClick.AddListener(ClientConnect);
        }
        obj = GetByName("SendButton");
        if (obj != null)
        {
            SendButton = obj.GetComponent<Button>();
            SendButton.onClick.AddListener(SendMsgBtn);
        }
        obj = GetByName("DisconnectBtn");
        if (obj != null)
        {
            DisconnectBtn = obj.GetComponent<Button>();
            DisconnectBtn.onClick.AddListener(DisconnectButton);
        }
        NameInputField = GetByName("NameInputField").GetComponent<InputField>();
        CurClientNum = GetByName("CurClientNum").GetComponent<Text>();
        ChatTalk = GetByName("ChatTalk").GetComponent<Text>();
        ChatInputField = GetByName("ChatInputField").GetComponent<InputField>();
        NameChat = GetByName("NameChat").GetComponent<Text>();
        ChatPanel = GetByName("ChatPanel").GetComponent<Image>();
        CreatePanel = GetByName("CreatePanel").GetComponent<Image>();
        DisconnectText = GetByName("DisconnectText").GetComponent<Text>();
    }
	
	// Update is called once per frame
	void Update () {
	    if (isChat)   //在可以聊天的时候进行处理
	    {
	        ChatTalk.text = textRecord;  //聊天框内现实的内容
	        string txt = "";

          foreach (var s in names)   
	        {
	            txt += s + "\n";
	        }
	        NameChat.text = txt;
	        textToSend = ChatInputField.text;   
	    }
	    else   //默认记录名字
	    {
	        myName = NameInputField.text;
	    }
	}

    void SendMsgBtn()   //发送消息按钮事件
    {
        networkView.RPC("SendText", RPCMode.All, myName+":"+textToSend);
        ChatInputField.text = "";
    }

    void DisconnectButton() //关闭聊天按钮事件
    {
        ChatPanel.gameObject.SetActive(false);
        CreatePanel.gameObject.SetActive(true);
        Network.Disconnect();
        isChat = false;
    }

    void ClientConnect()  //客户端连接服务器
    {
        NetworkConnectionError error = Network.Connect(IP, Port);
        if (error == NetworkConnectionError.NoError)
        {
            DisconnectText.text = "关闭聊天室";
            CreatePanel.gameObject.SetActive(false);
            ChatPanel.gameObject.SetActive(true);
            isChat = true;
            print("连接成功！");
        }
    }

    void OnConnectedToServer() //连接服务器
    {
        print("连接到服务器");
        networkView.RPC("Server_Join", RPCMode.Server, myName);
    }

    void OnPlayerDisconnected(NetworkPlayer player)  //当客户端断开服务器时被调用的函数
    {
        string id = player.ToString();
        print(id+"离开");
        if (manlist.ContainsKey(id))
        {
            networkView.RPC("SendText", RPCMode.All, manlist[id]+"离开了聊天室");
            manlist.Remove(id);
        }
        Server_UpdateNameList();
    }

    void CreateServer()  //创建服务器
    {
        NetworkConnectionError error = Network.InitializeServer(10, Port, false);
        if (error == NetworkConnectionError.NoError)
        {
            print("服务器创建成功！");
            DisconnectText.text = "关闭聊天室";
            ChatPanel.gameObject.SetActive(true);
            CreatePanel.gameObject.SetActive(false);
            manlist.Add(Network.player.ToString(), myName);
            Server_UpdateNameList();
            isChat = true;
        }
    }

    void Server_UpdateNameList()  //服务器向客户端发送人员名单及聊天记录，供客户端更新显示
    {  
        //string textToSend = "";
        foreach (var item in manlist)
        {
            textToSend += item.Value + "|";
        }
        networkView.RPC("RPC_GetNameList", RPCMode.All, textToSend);
        networkView.RPC("RPC_ChatRecordUpdate", RPCMode.All, textRecord);
        CurClientNum.text = "当前连接人数：" + (Network.connections.Length + 1);
    }

    [RPC]  //人员名单
    void RPC_GetNameList(string content, NetworkMessageInfo info)
    {
        string[] ss = content.Split('|');
        names.Clear();
        names.AddRange(ss);
    }

    [RPC]  //聊天记录
    void RPC_ChatRecordUpdate(string content)
    {
        textRecord = content;
        CurClientNum.text = "当前连接人数：" + (Network.connections.Length + 1);
    }

    [RPC]  //当客户端连接成功时向服务器发送的消息
    void Server_Join(string client_name, NetworkMessageInfo info)
    {
        string id = info.sender.ToString();
        print("Joined:"+id);
        networkView.RPC("SendText", RPCMode.All, client_name+"加入了聊天室");
        manlist.Add(id, client_name);
        Server_UpdateNameList();
    }

    [RPC]  //发送文本
    void SendText(string content, NetworkMessageInfo info)
    {
        textRecord += content + "\n";
    }
}

```

运行后输入名称xiayifan然后创建服务器则会显示如下效果：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/NetWorkView/images/Image3.png)

发布一个程序当Client使用，输入名称shylock点击连接服务器，然后可以进行聊天通信，效果如下：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/NetWorkView/images/Image4.png)

