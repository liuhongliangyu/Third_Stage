# LitJson解析JSON

## 常用的JSON库简介

**不推荐**

* Json.NET .NET自带的JSON库，缺点:比较臃肿

* MiniJson 缺点：不支持自定义的序列化

* LitJson 缺点：IOS平台支持不太友好，制作Android平台可以考虑使用

**推荐**

* MyJson

https://github.com/lightszero/myjson

* SimpleJson

http://blog.csdn.net/kakashi8841/article/details/21877131

**强烈推荐**

* JsonFX

## 1. LitJson配置

将库脚本或动态库（LitJson.dll）置在assets目录下的Plugins文件夹中，因为Plugins文件夹中脚本会先运行。

## 2. JSON的生成

（1）使用JsonData生成普通的JSON

```C#
JsonData data = ew JsonData();
data["name"] = "zhangsan";
data["age"] = 18;
data["sex"] = "男";
string json = data.ToJson();
print(json);

 ---------------运行结果-----------------
      {"name":"zhansan","age":28,"sex":"male"}     
```

（2）使用JsonData生成嵌套结构JSON

```C#
JsonData data = new JsonData();
data["name"] = "zhangsan";
data["age"] = 28;
data["sex"] = "男";
string json2 = data.ToJson();
print(json2);

        ---------------运行结果-----------------
      {"name":"zhansan","info":{"sex":"male","age":28}}   
```

（3）使用JsonMapper生成JSON

【Player.cs】

```C#
using System.Cllections;
public class Player
{
  public strign name {get; set; }
  public int age {get; set; }
  public strign sex {get; set; }
}
```

将Player对象转换为JSON格式

```C#
Player player = new Player();
player.name = "xiaoqiang";
player.age = 18;
player.sex = "男";
string json = JsonMapper.ToJson(player);
print(json);
```

## 使用JsonWriter生成JSON

1. 对象类型JSON

```C#
JsonWriter writer = new JsonWriter();
writer.WriterObjectStart();   //开始写JSON，相当于左花括号"{"
writer.WrutePropertyName("name"); //键
writer.write("xiaoqiang");  //值，与前一个键组成一对键值对
writer.WritePropertyName("age");
writer.write(18);
writer.WritePropertyName("sex");
writer.Write("男");
writer.WriterObjectEnd();   //结束写JSON，相当于右花括号"}"
print(writer.ToString());

	------------输出结果----------
   {"name":"xiayifan","age":18,"sex":"男"}
```

2. 解析JSON

（1） 使用JSonMapper解析上述JSON

```CE
JsonData jsonData = JsonMapper.ToObject(json2);  //(2) 使用JsonData生成嵌套结构JSON中的json2
Debug.Log(jsonData2["name"] + "  " + data2["info"]["sex"]);

    ---------------运行结果-----------------
    zhansan male
```

(2)使用JsonMapper解析JSON数据给指定对象

```C#
   string json='{"name":"zhansan","age":28,"sex":"male"}';
   Player player=JsonMapper.ToObject<Player>(json);
   Debug.Log("name="+player.name+" age="+player.age);

   name=zhansan age=28
```

(3)遍历JsonData的key和value

由于JsonData实现了IDictionary接口，所以可以使用Foreach进行遍历

```C#
JsonData jd = new JsonData();
jd["name"] = "张三";
jd["age"] = 35;
IDictionary dic=jd as IDictionary;
foreach (string key in dic.Keys)
{
	print(key);
}
```

练习：

1.利用JSON文件描述你平常所玩游戏信息，游戏信息包括游戏名，游戏类型，游戏物品的描述。

2.请使用JSON描述以下的商品的信息。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/JSON/LitJson/images/Image.jpg)


# 自作练习

```C#
using UnityEngine;
using System.Collections;
using LitJson;
using System.IO;
using System.Text;

public class CreateJSON : MonoBehaviour {

	// Use this for initialization
	void Start () {
        //CreateJson();
        //CreateJson_2();
        //CreateJson_3();
        //CreateJson_4();
        //CreateJson_5();
        //CreateJson_6();
        //CreateJson_7();
        //CreateJSON_8();
        JSON();

	}

    void JSON()
    {
        string PhysicsPath = "G:/Unity/MySQL/Assets/Plugins/stage1_1.json";   //JSON文件的物理路径（在磁盘中的路径）
        StreamReader sr = File.OpenText(PhysicsPath);  //使用流/写入器 打开指定文档
        StringBuilder jsonArrayText_tmp = new StringBuilder();  //定义StringBuilder可变字符序列
        string input = null;  //定义空字符串
        while ((input = sr.ReadLine()) != null)  //读取流中的文本内容，赋值给input字符串
        {
            jsonArrayText_tmp.Append(input);   //判断字符串中是否为空，如果不为空，将字符串追加到可变字符序列中
        }
        sr.Close();  //关闭流
        string jsonArrayText = jsonArrayText_tmp.ToString();   //将可变字符序列转换为字符串类型
        //string jsonArrayText = jsonArrayText_tmp.Replace(" ", "").ToString();
        print(jsonArrayText);
        JsonData data = JsonMapper.ToObject(jsonArrayText);
        print(data["id"]);
        print(data["title"]);
    }

    void CreateJson()
    {
        //使用JsonData生成普通的JSON
        JsonData data = new JsonData();
        data["name"] = "zhangsan";
        data["age"] = 18;
        data["sex"] = "男";
        string json = data.ToJson();
        print(json); 
    }

    //使用JsonData生成嵌套结构JSON
    void CreateJson_2()
    {
        JsonData data = new JsonData();
        data["name"] = "zhangsan";
        data["info"] = new JsonData();    //如果要使用数组类型，需要在创建一次JsonData的实例化
        data["info"]["sex"] = "男";
        data["info"]["age"] = 20;
        string json = data.ToJson();
        print(json);
    }

    //使用JsonMapper.ToJson()将player对象转换为JSON格式
    void CreateJson_3()
    {
        Player player = new Player();
        player.name = "lisi";
        player.age = 25;
        player.sex = "男";
        string json = JsonMapper.ToJson(player);
        print(json);
    }

    void CreateJson_4()
    {
        string json = @"{""name"":""xiaoqiang"", ""age"":15, ""sex"":""男""}"; //json和json1等价，只是json可以分行，而json1不能分行
        //string json1 = "{'name':'xiaoxia','age': 17, 'sex':'女'}";
        Player player = JsonMapper.ToObject<Player>(json);   //将JSON类型的字符串转换为Player对象类型
        print(player.name + "," + player.age + "," + player.sex);
    }
    void CreateJson_5()
    {
        string json = @"{""name"":""wangcai"", ""age"":30, ""sex"":""男""}";
        JsonData data = JsonMapper.ToObject(json);   //将json转换为对象类型，返回值为JsonData类型
        print(data["name"] + "," + data["age"] + "," + data["sex"]);
    }

    void CreateJson_6()
    {
        JsonWriter write = new JsonWriter();
        write.WriteObjectStart();
        write.WritePropertyName("name");
        write.Write("xiaoxin");
        write.WritePropertyName("age");
        write.Write(18);
        write.WritePropertyName("sex");
        write.Write("女");
        write.WriteObjectEnd();
        string json = write.ToString();
        print(json);
    }

    void CreateJson_7()
    {
        string json = @"{""name"":""wangcai"", ""age"":30, ""sex"":""男""}";
        JsonData data = JsonMapper.ToObject(json);
        IDictionary dic = data as IDictionary;   //将JsonData类型强制转换为IDictionary类型
        foreach (var key in dic.Keys)
        {
            print(key);
        }
        foreach (var value in dic.Values)
        {
            print(value);
        }
    }
    void CreateJSON_8()
    {
        string json = @"{""people"":[
        {""firstName"":""Brett"",""lastName"":""McLaughlin"",""email"":""aaa@163.com""},
        {""firstName"":""Jason"",""lastName"":""Hunter"",""email"":""bbbb@163.com""},
        {""firstName"":""Elliotte"",""lastName"":""Harold"",""email"":""cccc@163.com""}
    ]}";
        JsonData data = JsonMapper.ToObject(json);
        print(data["people"][0].ToJson());
    }
}

public class Player
{
    public string name { get; set; }
    public int age { get; set; }
    public string sex { get; set; }
}
```





