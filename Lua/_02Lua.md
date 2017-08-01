# 热更新Lua语法介绍

## Lua语言简介

### Lua是什么

Lua 是一个小巧的脚本语言。是巴西里约热内卢天主教大学（Pontifical Catholic University of Rio de Janeiro）里的一个研究小组，由Roberto Ierusalimschy、Waldemar Celes 和 Luiz Henrique de Figueiredo所组成并于1993年开发。 其设计目的是为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。Lua由标准C编写而成，几乎在所有操作系统和平台上都可以编译，运行。Lua并没有提供强大的库，这是由它的定位决定的。所以Lua不适合作为开发独立应用程序的语言。Lua 有一个同时进行的JIT项目，提供在特定平台上的即时编译功能。

## Lua编辑工具

### Lua编辑工具的下载和安装

* 1. Lua的官网 lua.org

* 2. Luaforwindows

http://luaforge.net/projects/luaforwindows/ 下载LuaforWindows

* 3. LuaStudio

LuaStudio 是一款轻量级的优秀的可调试的Lua IDE,非常方便的用于lua代码的编写和调试.

## 官方例子

安装完成后，在完成面板中勾选[Run a simple introduction toLua]选项，点击完成（Finish），就可以在控制台上看见大概有38个案例，如下

```Lua
--[[
print('hello worid')  --字符串类型可以使用单引号也可以使用双引号。
print("hello unity")]]

--[[ 这时多行注释，由双横线开头

紧跟着双中括号
在以双中括号结尾]]

-- print(111)  --也可以打印值类型
--[[
--变量，变量持有有类型的值，但是变量没有类型
a = 1
b = "abc"
c = {}
d = print]]
--[[
print(type(a))
print(type(b))
print(type(c))
print(type(d))]]
--[[
--变量名由字母、数字、下划线组成，不能以数字开头
one_two_3 = 123
print(one_two_3)
--1_two_3 = 123 不能以数字开头，会报错]]
--[[
--下划线通常用于特殊值开始
print(_VERSION)  --打印Lua的版本信息]]
--[[
--Lua区分大小写，因此所有的变量名称和关键字必须是正确的
ab = 1
Ab = 2
AB = 3
print(ab,Ab, AB)]]

--[[Lua的关键字：and,break,do,else,elseif,end,false,for,function,if,in,nil,not,or,repeat,return,then,until,while
关键字不能被命名为变量名，但是它们的大写可以被命名为变量名]]
AND = 1
--print(AND)
--[[
--Strings 字符串
a = "single 'quoted' string and double \"quoted\"string inside"
b = 'single \'quoted\' string and double "quoted" string inside']]
--c = [[multiple line with 'single' and "double" quoted Strings inside.]]
--print(a)
--print(b)
--print(c)
--[[
--Assignments 作业，多个作业是有效的
a,b,c,d,e = 1,2,"three", "four",5
print(a,b,c,d,e)]]
--[[
--多行任务允许一行交换变量
 a = 1
 b =2
 print(a,b)
 a, b = b, a
 print(a,b)]]
--[[
 --Number 值
 a,b,c,d,e = 1,1.123,1E9,-123,0.008
 print("a= "..a, "b= "..b, "c= "..c, "d= "..d, "e = "..e)]]
--[[
--More Output 更多种输出方式
print"Hello from Lua!"
print("Hello from Lua!")

io.write("Hello from Lua")  --不换行，在本行继续输出
io.write("Hello from Lua!")]]
--[[
--Tables 表 表与Array相似，什么类型都可以存
a = {}  --空表
b= {1,2,3}  --一个纯数值的表
c= {"a","b","c"} --纯字符串的表
print(a,b,c)  --输出表的地址]]
--[[
address={}  --创建一个空表
address.Street = "Wyman Street"  --网表内添加元素
address.StreetNumber = 360
address.AptNumber = "2a"
address.City = "Watertown"
address.State = "Vermont"
address.Country = "USA"

for key,value in pairs(address) do  --遍历表
print(key,value)
end

print(address.StreetNumber, address["AptNumber"])  --打印表的具体内容]]
--[[
-- if statement if语句
a = 1
if a== 1 then
print("a is one")
else
print("a is not one")
end

--if else statement if else 语句
b = "happy"
if b == "sad" then
	print("b is sad")
else
	print("b is not sad")
end

--if elseif else statement if elseif 语句
c = 3
if c == 1 then
	print("c is 1")
elseif c == 2 then
	print("c is 2")
else
	print("c is not 1 or 2, c is "..tostring(c))
end]]
--[[
--Conditional assignment 条件赋值
a = 1
b = (a== 1) and "one" or "not one"
print(b)

--等价于
a = 1
if a == 1 then
	b = "one"
else
	b = "not one"
end
print(b)]]
--[[
--while语句
a = 1
while a ~= 5 do -- ~= 是不等于的意思
	a = a + 1
	io.write(a.." ")
end]]
--[[
--repeat until 语句
a = 0
repeat
	a = a+1
	print(a)
until a ==5]]
--[[
--for语句
for a = 1, 4 do io.write(a) end  --从1数到4 1个数 一数
print()
for a = 1, 6, 2 do io.write(a) end  --从1疏数到6，2个数 一数

--More for 语句
for key, value in pairs({1,2,3,4}) do print(key, value) end]]
--[[
--tables 表
a = {1,2,3,4,"five", "elephant", "mouse"}
for i, v in pairs(a) do print(i,v) end]]
--[[
--break 语句
a = 0
while true do
	a = a + 1
	if a == 10 then
		break
	end
end
print(a)]]

--functions 方法
function myFirstLuaFunction()
	print("My first lua function was called")
end
myFirstLuaFunction()

--More Functions
function mySecondLuaFunction()
	return "string from my second function"
end
a = mySecondLuaFunction()
print(a)

function myFirstLuaFunctionWithMultipleReturnValues(a,b,c)
	return a,b,c, "My first lua function with multiple return values", 1, true
end
a,b,c,d,e,f = myFirstLuaFunctionWithMultipleReturnValues(1,2,"three")
print(a,b,c,d,e,f)

--变量范围和方法  所有变量都默认是全局的
b = "global"
function myfunc()
	local b = " loacal variable"
	a = "global variable"
	print(a, b)
end
myfunc()
print(a,b)

--Formatted printing 格式化的印刷  printf的实现
function printf(fmt, ...)
	io.write(string.format(fmt, ...))
end
printf("Hello %s from %s on %s \n", os.getenv"USER" or "there", _VERSION, os.date())

-- Math 数学运算函数
--[[Math function:
Math.abs, math.acos,math.asin, math.atan,math.atan2,
math,ceil, math.cos, math.cosh, math.deg, math.exp, math.floor,
math.fmod, math.frexp, math.huge, math.ldexp, math.log, math.lo10,
math.max, math.min,math.midf,math.pi,math.pow, math.rad,
math.random, math.randomseed, math.sin, math.sinh, math.sqrt,math.tan, math.tanh
]]
print(math.sqrt(9), math.pi)

--string  函数
--[[String function:
string.byte, string.char, string.dump, string.find, string.format,
string.gfind, string.gsub, string.len, string.lower, string.match
string.rep, string.reverse, string.sub, string.upper]]
print(string.upper("lower"), string.rep("a", 5), string.find("abcd", "cd"))

--table 放方法
-- table.concat, table.insert, table.maxn, table.remove, table.sort

a = {2}
table.insert(a, 3)
table.insert(a, 4)
table.sort(a, function(v1, v2) return v1 > v2 end)
for i, v in ipairs(a) do print(i, v) end

-- input/output 输入输出
--[[io function
io.close, io.flush, io.input, io.lines, io.open, io.output, io.popen,
io.read, io.stderr, io.stdin, io.stdout, io.tmpfile, io.type, io.write,
file:close, file:flush, file:lines, file:read,
file:seek, file:setvbuf, file:write]]
print(io.open("file doesn't exist", "r"))

-- operating system facilities 操作系统设施
--[[os function:
os.clock, os.date, os.difftime, os.execute, os.exit, os.getenv,
os.remove, os.rename, os.setlocale, s.time, os.tmpname]]
print(os.date())
--这里有大概36个例子
```

## 编辑第一个Lua程序

### Hello World

* 1. 找到luaforwindows的安装目录，找到SciTE

* 2. 打开SciTE，写入第一行Lua代码， 如：

```Lua
print("Hello World")
local a = 1
local b = 2
```

打印多个元素的内容 print(a,b)

* 3. 保存代码，保存为HelloWorld.lua 格式，必须有.lua后缀

* 4. 运行代码

### Lua中的关键字

下面列出了一些在Lua中的保留字。这些保留的字不可以被用作常量或变量，或任何其它标识符。

**and break do else elseif end false for function if in local nil not or repeat return then true until while**

**Lua的保留字如下：关键字不能当做标示符。Lua大小写敏感。**

### 注意事项

* 1. print()是Lua内置的方法

* 2. 在Lua中字符串用""或者’’都可以表示

* 3. Lua中每一条语句后面是没有;号的

* 4. 单行注释

```lua
--注释内容
```

* 5. 多行注释 

```lua
--[[1.注释
2.注释
3.注释]]
```

### 如何定义变量

#### Lua中定义变量

Lua是一个弱类型语言，不具有强类型语言的特点，比如：num = 100，这里定义一个全局变量叫做num，赋值为100.

在Lua中定义一个变量是没有类型的，根据存储什么数据，来定义是什么类型，在变量的命名中不能以数字开头，尽量避免下划线加大下字母开头，这种格式一般被Lua自身保留，推荐使用C#中的命名规范和驼峰命名

### Lua中的5中变量

Lua中的变量如下：

* 1. nil表示空数据，等同于null

* 2. boolean 布尔类型，存储true和false

* 3. string 字符串类型，字符串可以用双引号也可以使用单引号表示

* 4. number小数类型（Lua中没有整数类型）

* 5. table表类型

myTable = {34,,34,2,342,4}

myTable[3]

* 我们可以使用type()来取得一个变量的类型

```lua
  m  =3
  print(type(m))

  o  = "32rer"
  print(type(o))

  n  =nil
  print(type(n))
```

#### 局部变量和全局变量

##### 局部变量和全局变量注意事项

默认定义的变量都是全局的，定义局部变量需要在前面加一个local；
在代码块中声明的局部变量，只会在当前作用域之类起作用,在lua中重复定义不会报错.

```lua
temp = 34 –- 全局
local temp = 345 -– 局部
```

```lua
condition = true
x = 10
if true then
    local x                -- local to the "then" body
    x = 20
    print(x + 2)
else
    print(x)                --> 10  (the global one)
end

if false then
    local x                -- local to the "then" body
    x = 20
    print(x + 2)
else
    print(x)                --> 10  (the global one)
end
print(x)                    --> 10  (the global one)
```

### Lua支持的运算符

Lua支持的运算符有：

* 1. 算数运算符 + - * / % (Lua中没++ – 这样是运算符)

* 2. 关系运算符 <= < > >= == ~=

* 3. 逻辑运算符 and or not 分别表示 与 或 非（类似于C#中的 && || !）

a and b:当a为真时返回b,当a为假时,返回a <=> 条件表达式 a?b:a
　　
a or b:当a为真时返回a, 当a为假时返回b <=>条件表达式 a?a:b

not a:当a为真时返回假,当a为假时返回真 <=>条件表达式 a?false:true

```lua
    local a = false
    b = 5
    local c = a and b
    print (tostring(c))
```

### 流程控制 if语句

#### if语句的三种用法：

* 1. if语句

```lua
if [condition] then  -- 一定是与then 对应起来使用
end
```

* 2. if else语句

```lua
if [condition] then
else
end
```

* 3. if elseif else语句

```Lua
if [condition] then
elseif [condition]
else
end
```

* 4. if not 语句

```lua
if not false then
print(“ 如果不是”)
end
```

### 循环结构while循环

#### 循环结构while循环:

* 1. while语法结构

```lua
while [condition] do
end
```

* 2. 输出1到100

* 3. 实现1加到100

* 4. 遍历1-100中所有的奇数的和

### 循环结构repeat循环

#### 循环结构repeat循环:

（有点类似与C#中的do..while循环）

* 1. 语法结构

```lua
  repeat
  [code to execute]
  until [condition]

  local a = 1
  repeat
  	a = a +1
  	print (tostring(a))
  until( a>10 )
```

* 2. 输出1到100

* 3. 实现1加到100

* 4. 遍历1-100中所有的奇数的和

### for循环结构

#### for循环结构：

* 1. 语法结构

```lua
for index = [start],[end] do
[code to execute]
end
```

* 2. 输出1到100

* 3. 实现1加到100

* 4. 遍历1-100中所有的奇数的和

break可以终止循环 没有continue语法

### 函数（方法）

#### Lua中定义函数：

* 1. 如何定义函数

```lua
function 关键字表示函数开始,后接函数名和函数体 end表示函数结束
 function [function name](/resource/k25/03_引擎高级进阶/项目发布/Lua/Lua语法介绍/param_1,param_2)
 [function code]
end
```

定义一个函数用来求得两个数字的和

```lua
 function add(num1,num2)
    return num1+num2
 end

 local d = add(3,4)
```

一个函数返回多个值

```lua
function tear(a,b)
	return a+1,b+1
end

x,y = tear(1,2)
print(x,y)

x,y,z = tear(1,2)
print(x,y,z)
```

### 标准库(标准函数)

#### 内置标准函数

* Lua内置提供了一些常用的函数帮助我们开发

* 1. 数学处理的math相关函数

* 2. 字符串处理的string相关函数

* 3. 表处理的table相关函数

* 4. 文件操作的io相关函数
  
```lua
  file = io.open("D:\\1\\1.txt","w+")
  file:write("32432","a")
  file:close()
```

### 数学运算函数

#### 数学运算函数

* math.abs(x) 返回x的绝对值。(整数/浮动)

* math.cos(x) 返回x的余弦值 (假定为弧度)

* math.max(x，…) 返回参数的最大值

* math.maxinteger(x) 一个整数的最大值为整数

* math.min(x,…) 返回参数的最小值

* math.sqrt (x) 返回x的平方根

等等。。。

### 字符串处理相关函数

#### 字符串处理相关函数

* string.len(s) 接受一个字符串,并返回它的长度。

* string.lower(s) 接收一个字符串并返回该字符串的副本与所有大写字母改为小写

* string.upper（s） 接收一个字符串并返回该字符串的副本与所有小写字母改为大写。

* string.format(s)

```lua
str = string.format("字符串：%s\n整数：%d\n小数：%f\n十六进制数：%X","qweqwe",1,0.13,348)
```

* string.match(s) 对目标字符串进行匹配，并找到匹配的字符段

```lua
print(string.match("hello lua", "lua"))
print(string.match("data :2017/6/2", "%d+/%d+/%d+"))  --按照模式进行匹配
print(string.match("lua2 lua3", "lua%d", 2)) -- 匹配后面这个字符串
```

* .. 字符串相加

* tostring() 把一个数字转化成字符串 如 local d = tostring(s)

* tonumber() 把一个字符串转化成数字 local s = tonumber(d)

在lua中有默认的字符串和数字的自动转换:

```lua
print("10"+1)  --  11  number类型 ，字符串类型和数类型直接想加就转换成数字类型
local b = 10+"string" -- 错误  不能这样相加
```

* table的赋值

```lua
myTable[3]=34  当键是一个数字的时候的赋值方式
myTable["name"]="newbieol" 当键是一个字符串的赋值方式
myTable.name = "shylock"当键是一个字符串的赋值方式
```

* 3. table的访问

```lua
myTable[3]  当键是数字的时候，只有这一种访问方式
myTable.name 当键是字符串的时候有两种访问方式
myTable["name"]
```

* 4. table的第一种创建方式

```lua
myTable = {name="shylock",age=18,isMan = true}--	（表创建之后依然可以添加数据） 数据访问
myTable.name
myTable["name"]
```

* 5. table的第二种方式(类似数组的使用)
  
  ```lua
  myTable = {34,34,34,3,4,"sdfdsf"}
  ```
  
当没有键的时候，编译器会默认给每一个值，添加一个数字的键，该键从1开始

### 表的遍历

#### 表的遍历两种方式：

* 1. 如果是只有数字键，并且是连续的可以使用下面的遍历

```lua
for index = 1,table.getn(myTable) do
	[code to execute]
end

local num = {1,3,43,43534,43,435,5} --默认是连续的
for index = 1,table.getn(num) do
	print ( tostring( num[index]) )
end

print ( "\n" )

local num = {[2]=1,[9]=3,43,43534,43,435,5} --非连续的


print ( "\n" )
```

* 2. 表中通过pairs 和 ipairs进行遍历:

```lua
for index,value in pairs(myNames) do
    print(index,value)
end
print ( "\n" )

local tt =    
{    
    [1] = "test3",    
    [9] = "test4",    
    name = "test5"    
}    

for i,v in ipairs(tt) do    -- 当index不连续的时候或不是数字值时，不能完全遍历table中的值
    print( tt[i] )    
end  

for i,v in pairs(tt) do    --  当index不连续的时候，也可以完全遍历table中的值
    print( tt[i] )    
end
```

### table 表

#### Lua中的table — 表

* lua中表的特点

1. table 是 Lua 的一种数据结构用来帮助我们创建不同的数据类型，如：数字、字典等。

2. Lua table 使用关联型数组，你可以用任意类型的值来作数组的索引，但这个值不能是 nil。

3. Lua table 是不固定大小的，你可以根据自己需要进行扩容。

**在Lua中的table类似C#中的字典，其实就是一个 key-value键值对的数据结构。**

* table的创建
  
  ```lua
  myTable = {}
  表名后面使用{}赋值，表示一个空的表
```

### 表相关的函数

#### 表相关的函数

* 1. table.concat 把表中所有数据连成一个字符串

```lua
local string_concat = {"string", "int", "char", "float", "double"}  
local _tempStore_string = table.concat(string_concat,"\n",3);  -- 最后一个参数表示开始的下标
print(_tempStore_string)
```

* 2. table.insert 向指定位置插入一个数

```lua
local string_concat = {"string", "int", "char", "float", "double"}  
table.insert(string_concat,5,"3243");  
for index,value in pairs(string_concat) do
    print(index,value)
end
```

* 3. table.remove 移除指定位置的数据

```lua
local string_table = {"string", "int", "char", "float", "double"}  
table.remove(string_table,4);  
for index,value in pairs(string_table) do
    print(index,value)
end
```

* 4. table.sort 排序

```lua
local string_table = {1,4,2,5,3}    
for index = 1,table.getn(string_table) do
	print ( tostring( string_table[index]) )
end

print ( "\n" )
table.sort(string_table);

for index = 1,table.getn(string_table) do
	print ( tostring( string_table[index]) )
end

print ( "\n" )
```

#### 通过表来实现面向对象

* 通过表来实现面向对象

```lua
Buff={life=100}  申明Buff 表并定义life对象

function Buff.getBuff(self,a)
  self.life = self.life+a
end

function Buff:getBuff(a)
  self.life = self.life+a
end

Buff:getBuff(80) -- 冒号的调用方式
print(Buff.life)

Buff.getBuff(Buff,80) -- 点号的调用方式
print(Buff.life)

--定义并声明对象中的方法
```

# 练习

1. 举例写出包含常见控制型语句的程序,比如if/for/repeat/break ,思考如何实现continue的功能

```lua
--[[
for i = 1, 10 do
	while true do
		if i == 5 then
			break  //当i=5时，跳出while循环
		end
		print(i)
	break  //当i!=5时，跳出while循环
	end
end]]

a =0
repeat   //相当于C#中的do..while语句
	a = a + 1
	while true do
		if a == 5 then
			break
		end
	print(a)
	break
	end
until(a > 10)
```
