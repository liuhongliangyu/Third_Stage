# 热更新Lua和.Net相互调用

## 什么是LuaInterface?

* LuaInterface包括两个核心库一个是LuaInterface.dll,一个是Luanet.dll，我们可以通过LuaInterface完成Lua和C#(CLR)之间的互相调用

## 在C#中执行访问Lua代码

* 打开VS，创建新的项目（LuaText），在解决方案中，右击LuaText项目下的引用，选择添加引用，在控制面板中，点击浏览，找到luaforwindows的安装路径->clibs文件夹->LuaInterface.dll应用扩展程序，选中后，点击添加，点击确定

* 添加完成后，就可以在VS中引用LuaInterface命名空间了。这样就可以在VS中对Lua中的内容进行调用

* 打开我们新建的项目（LuaText），编写代码， 如下：

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using LuaInterface;

namespace LuaText
{
    class Program
    {
        static void Main(string[] args)
        {
            Lua lua = new Lua();  //创建Lua解析器
            lua["num"] = 2;  //定义一个num，double类型
            lua["str"] = "a string"; //定义一个字符串
            lua.NewTable("tab");   //创建一个空表 tab = {}
            Console.WriteLine(lua["num"]);
            double num = (double) lua["num"];
            string str = (string)lua["str"];
            Console.WriteLine(num+"..."+str);   //输出2...a string
            Console.ReadKey();
        }
    }
}
```

* DoString()方法：在C#中编写Lua语言脚本

```C#
lua.DoString("num = 2");
lua.DoString("string = 'a string'");
lua.DoString("function testFunc(string) print(string) end testFunc(string)");   //输出a string
//等同于
lua.DoString("str = 'a string' function testFunc(string) print(string) end testFunc(str)");
```

* DoFile() 方法：在C#中执行Lua文件

```C#
lua.DoFile("G://lua-5.3.4/Demo/VSToLua.lua");  //参数为Lua文件的绝对路径,执行VSToLua.lua文件的内容
```

## Lua和C#中的类型对应

```
Lua中              C#中
nil                null
string             System.String
number             System.Double
boolean            System.Boolean
table              LuaInterface.LuaTable
function           LuaInterface.LuaFunction
```

## 把一个C#方法注册进lua的一个全局方法

```C#
 static void Main(string[] args)
{
    Lua lua = new Lua();  //创建Lua解析器
    Program program = new Program();
     //将C#中的普通方法注册到lua中，方法必须是pubic
     // 第一个参数：注册到lua中，该方法的名字。第二个参数：注册哪个对象。第三个参数：这个对象里的方法 
    lua.RegisterFunction("NormalMethod", program, program.GetType().GetMethod("NormalMethod"));
    //调用注册到lua中的方法
    lua.DoString("NormalMethod()");//括号必须写
    //将C#中的静态方法注册到lua中,方法必须是public
    //lua.RegisterFunction("StaticMethod", null, program.GetType().GetMethod("StaticMethod"));
    lua.RegisterFunction("StaticMethod", null, typeof(program).GetMethod("StaticMethod"));  //也可以调用
    lua.DoString("StaticMethod()"); //括号必须写
}
public void NormalMethod()
{
    Console.WriteLine("NormalMethod Suc !!");
} 
 public static void StaticMethod()
{
    Console.WriteLine("StaticMethod Suc!!!");
}
```

## 在Lua中使用C#中的类

* 首先配置一下环境，在VS创建的项目（LuaText）的目录下找到bin->Debug文件夹，在该文件夹下创建script.lua文档，并将luanet.dll应用程序扩展拷贝到该文件夹下。

* 然后打开script.lua文档，进行编译（注意该文档的格式要求是ANSI编码格式）

```lua
require "luanet"

luanet.load_assembly("System")
Int32 = luanet.import_type("System.Int32")
--print(Int32)
num = Int32.Parse("3435")
print(num)
```

* 在VS中的LuaText中写代码：

```C#
 Lua lua = new Lua();  //创建Lua解析器
 lua.DoFile("script.lua");
```

* 运行VS，如果能够在控制台上输出3435，那么就说明，配置成功，lua与C#可以交互。

### 在Lua中使用C#中的方法和属性

* Lua文档

```lua
require "luanet"

luanet.load_assembly("System")
luanet.load_assembly("LuaText")  --加载程序集
Int32 = luanet.import_type("System.Int32")
Program = luanet.import_type("LuaText.Program") --加载程序集下的Program类
program1 = Program()
print(program1.name)
program1:TestMethod()   --调用普通方法（使用：）
program1.StaticMethod()  --调用静态方法（使用.）
```

* C#脚本

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using LuaInterface;

namespace LuaText
{
    class Program
    {
        static void Main(string[] args)
        {
            Lua lua = new Lua();  //创建Lua解析器
            lua.DoFile("script.lua");
        }
         public string name = "liu";

        public void TestMethod()
        {
            Console.WriteLine("TestMethod");
        }
         public static void StaticMethod()
         {
             Console.WriteLine("StaticMethod Suc!!!");
         }
    }
}
```

* 运行VS脚本，就可以在控制台上输出“liu”和“TestMethod”

### 在Lua中访问C#中的方法——特使情况

#### 1. 当函数中有out或ref参数时，out参数和ref参数和函数的返回值一起返回，并且调用时，out参数不需要传入C#函数定义

##### out参数

* Lua中

```lua
void,num = program1:TestOut( "xiaoqiang")
print(void,num)
```

* C#中

```C#
public void TestOut(string name, out int count)
{
    Console.WriteLine(name);
    count = name.Length;
}
```

##### ref参数

* lua

```lua
void, count = program1:TestRef("wangcai", 10)
print(void,count) 
```

* C#中

```C#
 public void  TestRef(string name, ref int count)
{
    Console.WriteLine(name);
    Console.WriteLine(count);
    count = name.Length;
}
```

#### 2. 当有重载函数的时候，调用函数会自动匹配第一个匹配的函数，这时可以使用get_method_bysig函数得到C#中指定类的指定参数的函数方法

* 当在C#脚本中有重载函数时，在lua中调用，如论怎么传参都会是匹配第一个函数，如下代码

```C#
 public void Method(int i)
{
    Console.WriteLine("Method Int i = " + i);
}
public void Method(string str)
{
    Console.WriteLine("Method string str = "+str);
}
```

```lua
program1:Method(111)
```

无论是使用上面的lua代码还是使用下面的lua代码

```lua
program1:Method("111")
```

在控制台上都会输出：Method Int i = 111

又或者是将C#代码的两个方法顺序变换一下，即：

```C#
public void Method(string str)
{
    Console.WriteLine("Method string str = "+str);
}
 public void Method(int i)
{
    Console.WriteLine("Method Int i = " + i);
}
```

那么无论是使用哪个lua脚本，都会输出：Method string str = 111

为了达到我们想要的目的，输入什么类型的参数，就会得到什么样类型的结果，我们在lua中可以使用get_method_bysig函数得到C#中指定类的指定参数的函数方法,即:

```lua
luaMethod = luanet.get_method_bysig(program1, "Method", "System.String")
luaMethod(1111)
--控制台输出：Method string str = 1111
```

```lua
luaMethod = luanet.get_method_bysig(program1, "Method", "System.Int32")
luaMethod(1111)
--控制台输出：Method Int i = 1111
```

这样，不论C#中的方法的顺序是什么，都可以得到我们想要的结果

