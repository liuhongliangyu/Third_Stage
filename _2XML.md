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

在上例中，正确嵌套的意思是：由于 "i" 元素是在 "b" 元素内打开的，那么它必须在 "b" 元素内关闭。

### （5） XML 的属性值须加引号

与HTML类似，XML也可以拥有属性（名称/值的对）。

在XML中，XML的属性值必须加引号。请研究下面的两个XML文档。第一个是错误的，第二个是正确的

第一种格式（错误）：

```XML
<note date=08/08/2008>
  <to>George</to>
  <from>John</from>
</note>
```

第二种格式（正确）：

```XML
<note date="08/08/2008">
  <to>George</to>
  <from>John</from>
</note>
```

在一个文档中的错误是，note元素中的date属性没有加引号

### （6）XML中的特殊字符

所有XML文档中的文本均会被解析器解析。

在处理XML数据时，特殊字符要特殊处理，不能喝节点字符混淆。

在XML中，一些字符拥有特殊的意义。

如果你把字符"<"放在XML元素中，会发生错误，这时因为解析器会把它当做新元素的开始。

这样会产生XML错误：

```XML
<message>if salary < 1000 then</message>
```

为了避免这个错误，请用实体引用泪代替"<"字符：

```XML
<message>if salary &lt; 1000 then</message>
```

在XML中，有5个预定义的实体引用：

* "&lt;" 代替 "<"  小于

* "&gt;" 代替 ">"  大于

* "&amp;" 代替 "&"  和号

* "&apos;"  代替 "'"  单引号

* "&quot;" 代替 """  引号

* 注：在XML中，只有字符"<" 和"&"确实是非法的。大于号是合法的，但是用实体引用来代替它是一个好习惯。

### （7）CDATA区段

术语CDATA指的是不应由XML解析器进行解析的文本数据（Unparsed Character Data）.

在XML元素中， "<"和"&"是非法的。"<"会产生错误，因为解析器会把字符解释为新的元素的开始。"&"也会产生错误，因为解析器会把字符解释为字符实体的开始。

某些文本，比如 JavaScript 代码，包含大量 “<” 或 “&” 字符。为了避免错误，可以将脚本代码定义为 CDATA。CDATA 部分中的所有内容都会被解析器忽略。

CDATA 部分由 “<![CDATA[“ 开始，由 ”]]>” 结束：

```XML
<script>
<![CDATA[
  function matchwo(a,b)
  {
  if (a < b && a < 0) then
    {
    return 1;
    }
  else
    {
    return 0;
    }
  }
]]>
</script>
```

在上面的例子中，解析器会忽略 CDATA 部分中的所有内容.


