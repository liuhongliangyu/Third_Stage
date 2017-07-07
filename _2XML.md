# XML语法结构

## 1. XML中的注释

在XML中编写注释的语法与HTML的语法很相似：

```XML
<!-- This is a comment -->
```

在XML中，空格会被保留，HTML会把多个连续的空格字符裁剪（合并）为一个

```XML
HTML:	Hello           my name is David.
输出:	Hello my name is David.
```

在XML中，，稳当的空格不会被删除。

## 2. XML语法规则

* XML文档必须有根元素

* 所有XML元素都必须关闭标签

* XML标签对大小写敏感

* XML必须正确地嵌套

* XML的属性值必须加引号

### (1）XML文档必须有根元素

XML文档必须有一个元素是所有其他元素的父元素。该元素称为根元素。

```XML
<root>
  <child>
    <subchild>.....</subchild>
  </child>
</root>
```

### （2）所有 XML 元素都须有关闭标签

在HTML中，经常会看到没有关闭的标签额元素：

```HTML
<p>This is a paragraph
<p>This is another paragraph
```

在XML中，省略关闭标签是非法的。所有元素都必须有关闭标签：

```XML
<p>This is a paragraph</p>
<p>This is another paragraph</p> 
```

注释：您也许已经注意到 XML 声明没有关闭标签。这不是错误。声明不属于XML本身的组成部分。它不是 XML 元素，也不需要关闭标签。

### （3） XML 标签对大小写敏感

XML 元素使用 XML 标签进行定义。

XML 标签对大小写敏感。在 XML 中，标签 <Letter> 与标签 <letter> 是不同的。

必须使用相同的大小写来编写打开标签和关闭标签：

```XML
<Message>这是错误的。</message
<message>这是正确的。</message>
```

注释：打开标签和关闭标签通常被称为开始标签和结束标签。不论您喜欢哪种术语，它们的概念都是相同的。

### （4）XML 必须正确地嵌套

在 HTML 中，常会看到没有正确嵌套的元素：

```HTML
<b><i>This text is bold and italic</b></i>
```

在 XML 中，所有元素都必须彼此正确地嵌套：

```XML
<b><i>This text is bold and italic</i></b>
```














