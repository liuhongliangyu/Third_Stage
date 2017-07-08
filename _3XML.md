# XML数据操作

## 创建XmlDocument

XmlDocument文件可以通过两种方式进行创建:

* 加载xml文件

* 通过字符串变量的形式

### 1. 加载xml文件

有以下XML文档data.xml.

```XML
<root>
  <data>
      <student id="1">
        <name>张三</name>
        <age>20</age>
      </student>
      <student id="2">
        <name>李四</name>
        <age>30</age>
      </student>
  </data>
</root>
```

当此文件放置在Assets目录下，加载代码如下

```XML
XmlDocument doc = new XmlDocument();
doc.Load(Application.dataPath + @"/data.xml");
```

### 2. 通过字符串变量的形式创建XmlDocument

```XML
XmlDocument doc = new XmlDocument();
string xmldata ="<root><data><student id='1'><name>张三</name><age>20</age></student><student id = '2'><name>李四</name><age>30</age></student></data></root>";
doc.LoadXml(xmldata);
```

## XML处理预备知识

在整个XML处理过中，需要使用到的几个类的关系图:

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/XML%E7%BC%96%E5%86%99%E8%A7%84%E8%8C%83/XML%E6%95%B0%E6%8D%AE%E6%93%8D%E4%BD%9C/images/20161205175000.jpg)

* 1 XMLElement 主要是针对节点的一些属性进行操作

* 2 XMLDocument 主要是针对节点的CUID操作

* 3 XMLNode 为抽象类，做为以上两类的基类，提供一些操作节点的方法

## 获取XML根元素

```XML
XmlDocument doc = new XmlDocument();
doc.Load(Application.dataPath + @"/data.xml");

//获取到根元素
XmlElement root = doc.DocumentElement;
```

## 选取的结点

关于结点的选取常用的方法有以下两种:

* SelectSingleNode 用于获取单个结点

* SelectNodes 用于获取多个同名结点

假设我们想获取所有student的信息,那必须要先访问到data结点，此时需要使用的方法是XmlElement.__SelectSingleNode__(结点标记名)

```XML
//选定students结点
   XmlNode data = root.SelectSingleNode("data");
```

当获取到data结点后，才能对所有的student信息进行访问。对于多个同名元素的访问，需要使用XmlNode.__SelectNodes__(结点名)

```XML
 XmlNodeList list=data.SelectNodes("student");
```

如果多个元素不同名，可以通过获取单个结点的方式单独访问，或者直接使用XmlNode.__ChildNodes__ 属性来获取所有的子结点。

```XML
XmlNodeList list=data.ChildNodes;
```

## 元素及属性访问

### 1. 获取元素.

【案例1】获取李四的年龄

```XML
//获取李四的年龄
XmlNode node = list[1].SelectSingleNode("age");
//输出此结点的xml内容
print(node.OuterXml);
//只获取age标签内的文本
print(node.InnerText);
```

### 2. 获取属性

对于属性的获取，可以使用以下方式:

* XmlElement的GetAttribute方法

* XmlNode的Attributes属性数组

```XML
//获取李四的序号
XmlNode node2 = list[1];
//将结点转为XmlElement
XmlElement xl = (XmlElement)node2;
//通过GetAttribute方法
print(xl.GetAttribute("id"));

//通过属性方式
print(node2.Attributes[0].Value);
```

