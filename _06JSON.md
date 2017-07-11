# JSON数据处理

## JSON简介

JSON（JavaScript Object Notation）是一种轻量级的数据交换格式。它基于ECMAScript的一个子集。JSON采用完全独立于语言的文本格式，但是也是用了类似于C语言家族的习惯（包括C、C++、C#、Java、JavaScript、Perl、Python等）。这些特性使JSON成为理想的数据交换语言。易于人阅读和编写，同时也易于机器解析和生成（一般用于提升网络传输速率）。

## JSON的语法规则

JSON语法是JavaScript对象表示语法的子集。

* 数据在键值对中

* 数据由逗号分隔

* 花括号保存对象

* 方括号保存数组

## JSON键值对

JSON数据的书写格式是：键值对。

键值对组合中的键写在前面（在双引号内），值写在后面（同样在双引号内），中间用冒号隔开：

```JSON
"firstName":"John"
```

这很容易理解，等价于这条JavaScript语句：

```C#
firstName = "John"
```

## JSON值

JSON的值可以是：

* 数字（整数或浮点数）

* 字符串（在双引号中）

* 逻辑值（true或false）

* 数组（在方括号内）

* 对象（在花括号内）

## JSON基础结构

JSON结构有两种结构，这两种结构就是对象和数组两种结构，通过这两种结构可以表示各种复杂的结构。

### 1. 对象

对象在JS中表示为“{}”括起来的内容，数据结构为{key:value,key:value,key:value,...}的键值对的结构，在面向对象语言中，key为对象属性，value为对应的属性值，所以很容易理解，取值方法法为 对象.key 获取属性，这个属性的类型可以是 数字、字符串、数组、对象等几种。

【参考案例1】

使用JSON数据表示iPhone手机的一些信息。

```JSON
{"name":"iPhone", "Price":3000}
```

#### 具体形似：

对象是一个无序的“键值对”集合

* 1. 一个对象以 “{” （左花括号）开始， “}” （右花括号）结束。

* 2. 每个“键”后面跟一个“：”（冒号）；

* 3. “键值对”之间用“，”（逗号）分割。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/JSON/JSON%E7%AE%80%E4%BB%8B/images/20161206174643.jpg)

【参考案例2】：表示一个对象

```JSON
{
  "姓名":"大憨",
  "年龄":24
}
```

【参考案例3】：表示一个外国人的名字

```JSON
{"firstName":"Brett","lastName":"Mclaughlin"}
```

在这组数据里，firstName和lastName是键（key），而Bretth和Mclaughlin是值（value）。

### 2. 数组

数组宅js中是中括号“[]” 括起来的内容，数据结构为["java","javaScript","vb",...],取值防暑和所有语言一样，使用索引获取，字段值的类型可以是数字、字符串、数组、对象几种。

当需要表示一个数值时，JSON不但能够提高可持续性，而且可以减少复杂性。如果使用JSON，就只需将多个花括号的记录分组在一起。

【参考案例4】：通过JSON数据存储三位外国人的名字及邮箱信息。

```json
{
  "people":[
    {"firstName":"Brett","lastName":"Mclaughlin","email":"aaa@163.com"},
    {"firstName":"Jason","lastName":"Hunter","email":"bbb@163.com"},
    {"firstName":"Elliotte","lastName":"Harold","email":"ccc@163.com"}
   ]
}
```

#### 具体形式

数组是值（value）的有序集合

* 1. 一个数组以 “[” （左中括号）开始，“]” （右中括号）结束。

* 2. 值之间使用 “,” （逗号）分割。

![](https://nts.newbieol.com/static/k25/03_%E5%BC%95%E6%93%8E%E9%AB%98%E7%BA%A7%E8%BF%9B%E9%98%B6/%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E5%8F%8AHTTP%E5%BA%94%E7%94%A8/JSON/JSON%E7%AE%80%E4%BB%8B/images/20161206174805.jpg)

```JSON
{
  "学生":[
    {"姓名":"小明", "年龄":23},
    {"姓名":"大憨","年龄":24}
  ]
}   
```

说明：次JSOn对象包括了一个学生数组，而学生数组中的值又是两个JSON对象。









