# TCP与UDP的区别

# TCP/IP协议栈的封装过程

![](http://s3.51cto.com/wyfs02/M01/54/A0/wKioL1SIB9bQaJBVAAGxIUxvNyc832.jpg)

以传输层采用TCP或者UPD、网络层采用IP、链路层采用Ethernet为例，可以看到TCP/IP中报文的封装过程如上图所示。用户数据经过应用层协议封装后传递给传输层，传输层封装TCP头部，交给网络层，网络层封装IP头部后，再交给数据链路层，数据链路层封装Ethernet帧头和帧尾，交给物理层，物理层以比特流的形式将数据发送到物理线路上。

## TCP Segment

Ethernet Frame

![](http://s3.51cto.com/wyfs02/M01/54/A2/wKiom1SIB0CA7CtDAADq-b6k7KM867.jpg)

### TCP协议概述

TCP为应用程序提供一种面向连接的、可靠的服务。

TCP的可靠性：

* 面向连接的传输

* 最大报文段长度

* 首部和数据的检验和

* 流式控制

### UDP协议概述

* UDP为应用程序提供面向无连接的服务。传输数据之前源端和目的端不需要建立连接。

* 不需要维持连接状态，收发状态等，因此服务器可同时向多个客户端传输相同的消息。

* UDP使用于对传输效率要求高的应用。

# TCP VS UDP

* UDP和TCP一样都使用IP作为网络层协议，TCP数据报被封装在一个IP数据包内。由于UDP不想TCP一样提供可靠的传输，因此UDP的报文格式相对而言较简单。

![](http://s3.51cto.com/wyfs02/M02/54/A0/wKioL1SICFyyZNXSAAFY4J3hgbU169.jpg)

参考资料：http://wangdy.blog.51cto.com/3845563/1588379
