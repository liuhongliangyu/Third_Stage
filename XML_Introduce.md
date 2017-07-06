# XML简介

## XML的起源和目的

XML是EXxtensible Markup Language的缩写，即可扩展标记语言。它是一种用来创建标记的标记语言。1996年，[万维网协会](http://www.w3c.org)开始设计一种可扩展的表记语言，于1998年2月，XML1.0称为W3C的推荐标准。

使用XML标记语言可以做到数据或者数据结构在任何编程语言环境下的共享。例如我们在某个计算机平台上用某种编程语言编写了一些数据或数据结构，然后用XML标记语言进行处理，那样的话，其他人就可以在其他的计算机平台上来访问这些数据或数据结构，甚至可以用其他的编程语言来操作这些数据或数据结构了。这就是XML标记语言作为一种数据交换元存在的价值。

## 什么是XML

* XML指可扩展标记语言（Extensible Markup Language）

* XML是一种标记语言，很类似HTML

* XML的设计宗旨是传输和储存数据，而非显示数据

* XML标签没有被预定义，您需要自行定义标签

* XML被设计为具有自我描述性

* XML是W3C的推荐标准

【参考案例】

下面是John写给George的便签，存储为XML格式数据

```XML
<?xml version = "1.0" encoding = "ISO-8859-1"?>
<note>
 <to>George</to>
 <from>John</from>
 <heading>Reminder</heading>
 <body>Don't forget the meeting!</body>
</note>
```

上面的这条便签具有自我描述性。他拥有标题以及留言，同时包含了发送者和接受者的信息。但是，这个XML文档仍然没有做任何事情。它仅仅是包装XML标签中的纯粹的信息。我们需要编写软件或者程序，才能发送、接收和显示这个文档。

## 












