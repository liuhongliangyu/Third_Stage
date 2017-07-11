# XML序列化和反序列化

## 序列化的实质

序列化是把一个对象持久到一个磁盘中的过程。应用程序的一部分，甚至另一个应用程序都可以反序列化对象，使它的状态与序列化之前相同。其中会用到System.Xml.Seriaization命名空间。反序列化可以理解为吧该过程倒过来再来一遍

## Xml序列化过程描述：

System.Xml.Serialization命名空间中最重要的类是XmlSerializer.要序列化对象，首先需要实例化一个XmlSerializer对象，指定要序列化的对象类型，然后实例化一个流/写入器对象，以把文件写入流/文档中。最后一步是在XmlSerializer上调用Serializer()方法，给它传递流/写入器对象和要序列化的对象
被序列化的数据可以为基本类型的数据、字段、数组，以及XmlElement和XmlAttribute对象格式的内嵌XML。

## XML反序列化过程的描述：

从XML文档中反序列化对象，应执行上述逆向过程。创建一个流/读取器对象和一个XmlSerializer对象，然后给DeSerializer()方法传递该流/读取器对象。这个方法返回序列化的对象，尽管它需要强制转换为正确的类型。

1.实际代码演示：序列化一个游戏物体（例如：玩家，怪物，NPC等）对象的实例，可以将它的数据（例如：位置，名字, 等级，血量，经验）序列化到XML中。

## XML序列化实例

* 1. 首先创建一个PlayerData类。如下所示

```C#
public class PlayerData   //  需要序列化的玩家数据类   
{
    public struct _Pos   // 结构类型
    {
        public float x,y,z;   // 表示位置的坐标点X,Y,Z;
    }
    public _Pos pos;

    private string name;
    public string Name      // 人物名称
    {
        get{ return name;}
        set{name =value ;}
    }
}
```

* 2. 在创建一个TestXmlSerializer类操作序列化过程。代码如下

```C#
using System.Xml;
using System.Xml.Serialization;
using System.IO;
public class TestXmlSerializer : MonoBehaviour
{
public GameObject Player;        //定义玩家对象
PlayerData data;                       // 定义玩家数据
string filepath;                         // 路径
void Start()
   {
      data = new PlayerData(）;      // 实例化对象
      filepath =  Application.dataPath+"/PlayerData.xml";   // 创建XML文件的路径
   }
void OnGUI()
    {
         if(GUI.Button(new Rect(10,10,100,20),"SaveData"))    
        {
            data.pos.x = Player .transform.position.x;
            data.pos.y = Player .transform.position.y;    
            data.pos.z = Player .transform.position.z;
            data.Name = Player .name;                                   // 将Plyer信息赋给玩家数据的对象；
            XmlSerializerData(data);                                        // 然后将数据序列化
        }
    }
void XmlSerializerData(data)
   {
        StreamWriter sr;                                                       //流/写入器对象
        FileInfo info = new FileInfo (filepath);                       //创建filepath路径下的文件对象
        print (info);
        if (!info.Exists) {                                                        // 判断路径是否存在，不存在则创建，存在则删除再创建
            sr = info.CreateText ();
        }else
        {
            info.Delete();
            sr = info.CreateText();
        }
        XmlSerializer xs = new XmlSerializer (typeof(PlayerData));       // XmlSerializer对象并且指定序列化类型
        xs.Serialize (sr, data);                                                              //  序列化方法（传递流/写入器对象，和玩家数据对象）
        sr.Close ();                                                                              //  关闭流
       }
}
```

## XML反序列化实例

* 1. 创建一个TestXmlDeSerializer类，那么其实反序列化的关键就在于DeSerializer方法的使用。代码如下：

```C#
using System.Xml;
using System.Xml.Serialization;
using System.IO;
public  class TestXmlDeSerializer: MonoBehaviour
{
public GameObject Player;        //定义玩家对象
PlayerData data;                       // 定义玩家数据
string filepath;                         // 路径
Vector3 TempPos;
void Start()
   {
      data = new PlayerData(）;      // 实例化对象
      filepath =  Application.dataPath+"/PlayerData.xml";   // 创建XML文件的路径
   }
void OnGUI
   {
     if (GUI.Button (new Rect (10, 40, 100, 20), "LoadData"))
        {
           data = DeSerializerData();                                // 接受返回的反序列化出来的的对象
           TempPos = new Vector3(data.pos.x,data.pos.y,data.pos.z);    将反序列化出来的数据（坐标）赋给临时坐标
           Player .transform.position = TempPos;             
           Player .name = data.Name;                             //  然后把该数据再转给现在的玩家
        }
    }
PlayerData DeSerializerData()
   {
         PlayerData playerdata = new PlayerData ();
         FileStream f = new FileStream (filepath,FileMode.Open);      //  打开filepath路径中的XML文件（文件流对象）
         XmlSerializer xs = new XmlSerializer (typeof(PlayerData));     // 指定需要操作的对象类型
         playerdata = (PlayerData)xs.Deserialize (f);                            //  反序列化成一个object对象然后强制转化成PlyerData类型
         print (playerdata.pos.x+","+ playerdata.pos.y+","+ playerdata.pos.z);  打印出序列化之前（反序列化之后）的 玩家数据
         return playerdata;          // 返回这个对象
     }
}
```

