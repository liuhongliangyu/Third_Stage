# ProtoBuf使用

## ProtoBuf简介

### Protocol Buffers定义

Protocol Buffers是Google公司开发的一种数据描述语言，类似于Xml能够将结构化数据序列化，可用于数据存储、通信协议等方面。它不依赖与语言和平台并且可扩展性极强。现阶段官方支持C++、JAVA、Python等三种语言，但可以找到大量的几乎涵盖所有语言的第三方扩展包。

protobuf-net地址：：https://github.com/mgravell/protobuf-net

### Protobuf优点

### 为什么不知是用XML？

同XML相比，Protocol buffers在粗劣化结构化数据方面有许多优点：

* 1. 更简单

* 1. 数据描述文件只需要原来的1/10至1/3

* 1. 解析数独是原来的20倍至100倍

* 1. 减少了二义性

* 1. 生成了更容易在编程中使用的数据访问类

* 1. 支持多种编程语言

### ProtoBuf性能分析

如下列图所示，ProtoBuf性能相对较好，应用领域包括：网络传输，配置文件，数据存储

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/Protobuf/images/Image.png)

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/Protobuf/images/Image1.png)

## Protobuf语言规则

### 指定字段类型

所有的字段都是标量类型：string、bool、int等，当然，你也可以为字段指定其他的合成类型，包括枚举（额怒merations）或者其他消息类型

### 分配标识符

在消息定义中，每个字段都有唯一的一个标识符。这些标识符是用来在消息的二进制格式中识别各个字段的，一旦开始使用就不能再改。

* 注：[1,15]之内的标识号在编码的时候会占用一个字节。[16， 2047]之内的标识号则占用2个字节。所以应该为那些频繁出现的消息元素保留[1,15]之内的标识号。

* 切记：要为将来有可能添加的、频繁出现的标识号预留一些标识号。

最小的标识号可以从1开始，最大的到220 - 1， or536,870,911。不可以使用其中的[19000 - 19999]的标识号，ProtoBuf协议协议实现中对这些进行预留。如果非要在.proto文件中使用这些预留标识号，编译时就会报警。

### 指定字段规则

所指定的消息字段修饰符必须是如下之一：

* required:不可增加或删除的字段，必须初始化；

* optional:可选字段，可删除，可以不初始化；

* repeated:可重复字段（对应的C#里面的List）。

### 标量数值类型

一个标量消息字段可以含有一个如下的类型——该表格展示了定义与.proto文件中的类型，以及与之对应的、在自动生成的访问类中定义的类型：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/Protobuf/images/Image3.png)

### 添加注释

向.proto文件添加注释，也跟C#语法一样（//）的语法格式，如：

```
message SearchRequest
{
    required string query =1;
    optional int32 page_number =2; //最终返回的页数
    optional int32 result_per_page =3; //每页返回的结果数   
}
```

### 枚举类型

```CS
message SearchRequest
{
required string query = 1;
optional int32 page_number = 2;
optional int32 result_per_page = 3 [default = 10];
enum Corpus {
UNIVERSAL = 0;
WEB = 1;
IMAGES = 2;
LOCAL = 3;
NEWS = 4;
PRODUCTS = 5;
VIDEO = 6;
}
optional Corpus corpus = 4 [default = UNIVERSAL];
}
```

### 嵌套类型

```
package Cmd;
	message Person
	{
	    message Address
	    {
	         required string Line1 = 1;
	         required string Line2 = 2;
	    }
    	required int32 Id = 1;
	    required string Name = 2;
	    required Address address = 3;
  }
```

### 可以多重嵌套

```
package Cmd;
message Outer {
  message MiddleAA{
    message Inner {
      required int64 ival = 1;
      optional bool  booly = 2;
    }
  }
  message MiddleBB {
    message Inner {
      required int32 ival = 1;
      optional bool  booly = 2;
    }
  }
}
```

### 引用外部定义的消耗类型

* 注：没做出来...
```
UserData.proto
	message UserItem
	{
		required string userId = 1;
		required string userName = 2;
	}
Test.proto
	package Cmd;
	import "UserData.proto";
	message Test
	{
		optional string name = 1;
		required UserItem userItem = 2;
	}
```

### ProtoBuf例子

protobuf应用于C#Model的实际案例，代码如下所示：

```
Package Cmd;
message Person
{
	optional string Child = 1;
	repeated string Parent = 2;
	required string Name = 3;
	required bool Gender = 4;
	required string Msg = 5;
}
```

紧接着将Protobuf格式的文件转化成我们熟知C#的Model，

步骤如下：

首先在dos下cd到protogen.exe然后再输入

protogen.exe -i:PersonInfo.proto -o:PersonInfo.cs效果如下图：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/Protobuf/images/Image2.png)

* 注：在Cmd控制台中，将protogen.exe应用程序拖进cmd中+" -i:"+再将上面的文件拖到cmd中+" -o:"+再将上面的文件拖到cmd中，将文件的后缀".proto"改为".cs"。

## protobuf序列化和返序列化

### protobuf序列化

是指将结构化的数据按一定的编码规范转成指定格式的过程

### protobuf反序列化

是指将转成指定格式的数据解析成原始的结构化数据的过程

### 控制台演示序列化和返序列化的案例

打开VS创建一个新项目ProtoBuf_Test

在这个项目下添加引用：在解决方案中右键点击项目ProtoBuf_Test->添加->引用->点击浏览，找到

ProtoGen文件夹下的protobuf-net.dll应用程序扩展。

首先创建一个Person类如下图：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/Protobuf/images/Image4.png)

```C#
using System.IO;
using System.Text;
using ProtoBuf;
namespace protobuf_test
{
    class ProtobufHelper
    {
        /// <summary>
        /// 序列化成string
        /// </summary>
        /// <typeparam name="T"></typeparam>
        /// <param name="t"></param>
        /// <returns>返回字符串</returns>
        public static string Serialize<T>(T t)
        {
            using (MemoryStream ms = new MemoryStream())
            {
                Serializer.Serialize<T>(ms, t);
                return Encoding.UTF8.GetString(ms.ToArray());
            }
        }
        /// <summary>
        /// 序列化到文件
        /// </summary>
        /// <typeparam name="T"></typeparam>
        /// <param name="path"></param>
        /// <param name="data"></param>
        public static void Serialize<T>(string path, T data)
        {
            using (Stream file = File.Create(path))
            {
                Serializer.Serialize<T>(file, data);
                file.Close();
            }
        }
        /// <summary>
        /// 根据字符串反序列化成对象
        /// </summary>
        /// <typeparam name="T"></typeparam>
        /// <param name="content"></param>
        /// <returns></returns>
        public static T DeSerialize<T>(string content)
        {
            using (MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(content)))
            {
                T t = Serializer.Deserialize<T>(ms);
                return t;
            }
        }
        /// <summary>
        /// 根据文件流返回对象
        /// </summary>
        /// <typeparam name="T"></typeparam>
        /// <param name="filestream"></param>
        /// <returns></returns>
        public static T DeSerialize<T>(Stream filestream)
        {
            return Serializer.Deserialize<T>(filestream);
        }

    }
}
```

然后利用封装好的ProtobufHelper(代码如下)类型进行序列化和反序列化， 代码如下：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/Protobuf/images/Image5.png)

控住台程序运行结果：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/Protobuf/images/Image6.png)

### Unity中使用Protobuf序列化和反序列化

将protobuf-net.dll应用程序扩展拖放到unity的Project面板中的Plugins文件夹中，将上述由.proto格式转换为.cs 的文件拖拽到Plugins中。

* 1. 创建一个PersonInfo.proto文件，添加内容如下：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/Protobuf/images/Image7.png)

* 2. 把PersonInfo.proto文件生成PesonInfo.cs文件，再将生成PersonInfo.cs文件打开浏览，如下图所示：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/Protobuf/images/Image8.png)

* 3. 完成Unity中代码，效果图如下：（注意：（ProtobufHelper类如上）此过程是序列化到文本中）

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/Protobuf/images/Image9.png)

Console控制台执行结果如下：

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/Socket%E9%80%9A%E4%BF%A1/Protobuf/images/Image10.png)







