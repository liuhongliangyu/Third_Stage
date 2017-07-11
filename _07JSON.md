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








