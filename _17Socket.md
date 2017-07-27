# Socket的使用

【例子】TCP聊天室

创建服务器

VS创建项目ServerSochet

代码如下

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Runtime.Remoting.Messaging;
using System.Security;
using System.Text;
using System.Threading;
using System.Threading.Tasks;


namespace ServerSochet
{
    class Program
    {
        private static List<ClientSocket> cSocketList = new List<ClientSocket>();
        static void Main(string[] args)
        {
            //1. 创建监听客户端连接的Socket                  寻址的Ip类型                  流                TCP协议
            Socket serverSocket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
            Console.WriteLine("服务器创建成功...");
            //2. 绑定服务器IP和端口
            IPAddress ipAddress = IPAddress.Parse("127.0.0.1");  //将IP地址值转换为IPAddress类型
            int port = 7788;   //端口号
            EndPoint point = new IPEndPoint(ipAddress, port);
            serverSocket.Bind(point);
            Console.WriteLine("服务器启动成功...");
            //3. 设定监听队列
            serverSocket.Listen(10);  //监听个数，即服务器允许的人数
            Console.WriteLine("服务器开始监听..");
            Console.WriteLine("等待客户连接...");
            //4. 等待客户连接
            while (true)
            {
                //等待用户连接如果用户连接则返回一个与用户通信的socket
                Socket newSocket = serverSocket.Accept();  //返回一个客户端的Socket
                if (newSocket != null)
                {
                    Console.WriteLine("服务器连接到一个新客户端");
                    Console.WriteLine("客户端IP："+ newSocket.LocalEndPoint);
                }

                ClientSocket CSocket = new ClientSocket(newSocket);
                cSocketList.Add(CSocket);  //将新生成的Socket对象添加到列表中
                
            }
            
        }

        //用来向客户端广播消息
        public static void BroadCastMessage(string msg)
        {
            var newSochetList = new List<ClientSocket>();
            for (int i = 0; i < cSocketList.Count; i++)
            {
                if (cSocketList[i].isConnect)
                {
                    cSocketList[i].SendMessage(msg);
                }
                else
                {
                 newSochetList.Add(cSocketList[i]);   
                }    
            }
            for (int i = 0; i < newSochetList.Count; i++)
            {
                cSocketList.Remove(newSochetList[i]);
            }
        }
    }

    //服务端用来和客户端交互的类
    public class ClientSocket
    {
        private Socket tcpS2CSocket;
        private Thread thread;

        public bool isConnect
        {
            get { return tcpS2CSocket.Connected; }
        }


        public ClientSocket(Socket tcSocket)
        {
            this.tcpS2CSocket = tcSocket;
            thread = new Thread(ReceiveMessage);
            thread.Start();
        }

        //5. 接收消息
        private void ReceiveMessage()
        {
            
            while (true)
            {
                //用来判断客户端的Socket是否还在连接状态
                if (tcpS2CSocket.Poll(10, SelectMode.SelectRead))
                {
                    tcpS2CSocket.Close();
                    break;
                }
                byte[] data = new byte[2048];
                int length = tcpS2CSocket.Receive(data);  //获取该消息的长度
                string message = Encoding.UTF8.GetString(data, 0, length);
                Console.WriteLine("接收到消息：" + message);
                //广播消息
                Program.BroadCastMessage(message);
            }
            
        }
        //6. 发送消息
        public void SendMessage(string message)
        {
            byte[] bytes = Encoding.UTF8.GetBytes(message);
            tcpS2CSocket.Send(bytes);
            Console.WriteLine("客户端向服务器发送消息："+message);
        }
    }
}

```

在Unity中搭建UI

* 创建2DUI，在UI下创建Panel。

  * 创建InputField，重命名为InputFieldChat，表示输入框
  
  * 创建Button，重命名为SendBtn，表示为发送文本按钮
  
  * 创建Text，重命名为ChatText，表示为聊天显示内容
  
写代码，挂载到画布（Canvas）上

```C#
using System;
using UnityEngine;
using System.Collections;
using System.Net;
using UnityEngine.UI;
using System.Net.Sockets;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading;

public class SocketClientChat : MonoBehaviour
{
    private InputField inputfieldChat;
    private Button sendBtn;
    private Text chatText;
    private string message = "";
    private Socket clientSocket;
    private Thread thread;
    private byte[] bytetemps = new byte[2048];

    void Awake()
    {
        inputfieldChat = transform.Find("Panel/InputFieldChat").GetComponent<InputField>();
        sendBtn = transform.Find("Panel/SendBtn").GetComponent<Button>();
        sendBtn.onClick.AddListener(SendMsg);
        chatText = transform.Find("Panel/ChatText").GetComponent<Text>();
    }
    // Use this for initialization
    void Start()
    {
        ConnectToServer();
        
    }

    void ConnectToServer()
    {
        //1. 创建与服务端连接的socket（负责接收和发送消息）
        clientSocket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);

        IPAddress ipadd = IPAddress.Parse("127.0.0.1");
        EndPoint point = new IPEndPoint(ipadd, 7788);
        //连接服务器
        clientSocket.Connect(point);
        print("连接到一个服务器");
        thread = new Thread(ReceiveMessage);
        thread.Start();
    }

    // Update is called once per frame
    void Update()
    {
        if (message != null && message != "")
        {
            chatText.text += message + "\n";
            message = "";
        }
    }

    //发送消息
    void SendMsg()
    {
        message = inputfieldChat.text;
        byte[] bytes = Encoding.UTF8.GetBytes(inputfieldChat.text);
        clientSocket.Send(bytes);
        print("客户端向服务器发送消息：" + inputfieldChat.text);
        inputfieldChat.text = "";
    }
    //接受服务器发送过来的消息
    public void ReceiveMessage()
    {
        while (true)
        {
            if (clientSocket.Connected == false)
            {
                break;
            }
            //获得该消息的长度
            int length = clientSocket.Receive(bytetemps);
            string str = Encoding.UTF8.GetString(bytetemps, 0, length);
            print("接受到的服务端的消息：" + str);
            //chatText.text = "\n" + str;
        }
    }

    void ShowText(string message)
    {
        chatText.text += message + "\n";
    }

}
```

实验，检查有木有错误，如果没有，可以发布。

发布之后，运行VS中的服务器脚本，然后运行发布的软件（可以同时运行多个软件），那么软件之间就可以进行交互了








