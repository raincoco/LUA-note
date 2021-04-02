# LUA脚本笔记

```lua
------------------------
--------数据类型---------
------------------------
--类型特征：Lua为弱类型语言，声明变量时不需要指定类型，变量的类型由变量内存储的数据决定即可直接赋值，无需声明。
--数据类型：数值类型number；布尔类型bool。


number = 100
bool = true
char = "字符串"

--字符串连接
char1 = "hello!"
char2 = "world!"
char3 = char1..char2

------------------------
--------分支结构---------
------------------------
-- if
-- then和end代替{} 
-- 只有if下才有then

A = 1
B = 2

if(A > B)
then
	print("A")
else
	print("B")
end

if(A > B)
then
	print("A")
elseif(A < B)
then
	print("B")
else
	print("A=B")
end

------------------------
--------循环结构---------
------------------------
-- 1、for循环

-- c++的for结构
for(i = 0;i <= 3;i++){
	printf("1\n");
}

-- lua的for结构
for i = 0, 3, 1 do
	print("1\n")
end

-- 2、while循环
while(A > B)
do
	print("A")
end

--3、类似于do..while的语句
repeat
	print("hi")
until(A > B)	
--当until中布尔表达式为false时循环执行

--Lua中的循环结构只有使用break关键字可以跳出 

-------------------------
----------函数-----------
-------------------------
--Lua是解析型语言，函数必须先声明，然后才可以调用 
function hello()
	print("hello!")
end

function add(A, B)
	return A+B
end

--函数可直接成为参数传递
function adds(A, B, func)
	return func(A, B)
end

C = 3 
D = 4
result = adds(C, D)
print(result)

-------------------------
----------数组-----------
-------------------------
--[[
长度：Lua数组长度不固定，可以给后续的索引位置继续赋值
索引：Lua数组索引从1开始
存储类型：可以存储多种类型的数据       
]]

--数组
array = { 10, true, '字符串' }

--获取数组长度
table.getn(array)

--历遍
for i = 1, table.getn(array), 1,do
	print(array[i]);
end

----------------------------
----------字符串------------
----------------------------
--转义字符：\
--字符串表现形式：
--用双引号包裹：“constant”
--用单引号包裹： 'constant'
--用双方括号包裹：[[constant]]

--字母大小写转换：
string.upper( 'str' )--字母全部转大写格式
string.lower( 'str' )--字母全部转小写格式

--字符串反转
string.reverse( 'str' )--将字符串进行位置反转

--字符串长度
string.len( 'str' )--字符串长度

--字符串替换
--tring.gsub(目标字符串，模式字符串，替换符串，[替换次数])

str = '12345abcde'
string.gsub(str，'abcde'，'ABCDE')--将str字符串中的abcde替换成ABCDE

--如果目标字符串中没有指定要替换的字符串，则返回整个目标字符串
string.gsub(str，'zzzzz'，'ABCDE')--str字符串中没有zzzzz，所以返回12345abcde

----------------------------
----------table表-----------
----------------------------
--1.table的基本定义：
--<1>.什么是 table?：table 是 Lua 语言中的一种“数据/代码结构”，可以用来帮助我们创建不同 的“数据类型”；Lua 语言中的数组其实就是table 类型.
--<2>.table 和数组的关系：table 是 Lua 语言中的一种“代码格式结构”，而 Lua 当中的数组就是使用table这种结构实现的。

--2.table的基础操作：
--<1>.声明和赋值：
--a. 初始化table：表名 = { }
mytable = {}--直接声明

--b. 给 table 赋值：
--数组方式，就是以角标方式进行赋值
mytable[1] = 10
mytable[2] = 'table'
print(myTable[1])

--键值对方式
myTable = {}  --直接声明
myTable['name'] = 'myName'
myTable['age'] = 18
myTable['isboy'] = true
print(myTable['name']) 
print(myTable['age'])
print(myTable['isboy'])

--c.迭代器方式遍历 table
for key, value in ipairs(表名) do   --如果是数组结构，用 ipairs 方法；如果是键值对结构，用 pairs 方法.
      print(key, value)
end
--[[
<2>.table的相关操作方法：

1.增加元素：table.insert(表名, [位置], 值)
往指定的位置增加元素，如果不写位置，默认往最后一个位置增加。

这个方式适合“数组模式”，不太适合“键值对模式”。
键值对就用：表名[‘键’] = 值 的方式添加即可。

2.移除元素：table.remove(表名, [位置])
如果不写位置，默认移除最后一个元素，如果位置值超出范围，不会报错，也不会有元素被移除。

这个方式适合“数组模式”，不能用于“键值对模式”。
键值对就用：表名[‘键’] = nil 的方式移除即可。

3.table 长度：table.getn(表名)， 返回 table 表的长度。

这个方式适合“数组模式”，不能用于“键值对模式”。
键值对就用：迭代器迭代，然后累加一个变量的方式获得长度。

table自带的操作函数大都只适用于数组模式，键值对模式相对麻烦点。
]]

----------------------------
-------Metatable元表--------
----------------------------
--1.元表的作用：
--元表（metatable）就是让两个表之间产生“附属”关系，只需要操作主表，就可以间接的操作元表。

--2.元表相关操作
--<1>.实例化两个普通表[表 A 和表 B]
tableA = {}
tableB = {}
--<2>.关联两个表[将表 B 设置成表 A 的元表]，需要用一个新的函数：setmetatable(表 A, 表 B)
setmetatable(tableA, tableB) --A是主表，B是子表
--<3>.getmetatable(表名)：如果表名有元表，就返回元表的类型和地址；如果没有元表，则返回一个 nil

--<4>__index元方法
--定义一个table t。
local t = { 
	name = "ann", 
} 
print(t.money); 
--访问t没有的字段money，返回nil。如果我们希望在访问一个table不存在的字段的时候，能进行一些自定义的操作，而不是返回mil呢？
--可以使用__index元方法

--例一
local t = { 
	name = "ann", 
} 
    
local mt = { 
	__index = function(table, key)   --定义__index，传入参数table和字段（t和age）
		print("调用了不存在的字段" .. key); 
    end 
} 

setmetatable(t,mt); 
--调用t的age字段，但是t没有该字段，故调用__index方法
print(t.age); 

--例二
local t = { 
	name = "ann",  
} 
    
local mt = { 

	__index = {       --mt的__index方法是一个表且中有age字符
		age = 18, 
    } 
} 
setmetatable(t,mt); 
    
print(t.age); 
--当t中不存在age字段，t调用元表mt的__index方法，调用他的age字段。

--[[
当调用table中不存在的想要的字段时，会调用table元表的__index元方法。
如果这个__index元方法是一个table的话，那么，就会在这个table里查找字段，并调用。
]]

--例三:索引
tableA = {}
tableB = {}
 
tableA['name'] = 'ann'
tableA['age'] = 18
tableB['grade'] = 3
tableB['number'] = 0001
 
setmetatable(tableA, tableB)   --将tableA设为主表，tableB为子表

--tableB将自己作为__index方法，当主表tableA调用grade、number字段找不到调用__index方法时，相当于到tableB中寻找这两个字段。
tableB.__index = tableB    --将tableB的索引赋给主表tableA

print(tableA.grade)        --此时tableA就可以通过tableB的索引访问其中的值
print(tableA['Bname'])     --也就是说tableB完全成为了tableA的一部分

--3、继承
--定义table student为基类
local student = { 
        name = "ann", 
        age = 18, 
        number = 00001, 
        
        tip = function() 
            print("I'm student")
        end 
} 
    
    local t1 = {} 
    local t2 = {} 
    
    local mt = {__index = student} --mt定义student表为__index元方法
    
    --mt作为t1，t2的元表
    setmetatable(t1, mt) 
    setmetatable(t2, mt) 
    
    --在t1表中调用number字段，t1中没有该字段，调用元表mt的__index方法，在student表中调用number字段
    print(t1.number)
    --t2.tip同理
    t2.tip() 

----------------------
-------类与对象--------
----------------------
--1.Lua的类实现步骤
--<1>类：就是初始化一个 table。
--<2>类中的字段：在 table 的{ }内进行定义，可以指定初始化值。
--<3>类中的方法：就是普通函数的语法格式，方法名的语法格式是“类名:方法名”,比如。
--<4>self 关键字：作用类似于 C#中的 this。

--2.对象的实例化
--<1>类的构造方法：
--student类的声明，有初始字段
student = {name, age, number}  

--构造方法，格式时固定的，也可传递构造参数
function student:New()   
    local obj = {}
    setmetatable(obj, student)  --设置主表
    student.__index = student   --设置子表的索引
    return obj                  --返回主表对象
end
--类中的方法
function student:info()
    print(self.number.." "..self.name)  --self的作用类似于C#中的this，特指这个类中的成员
end

--<2>实例化对象并初始化并使用
--创建对象stu，实例化student
stu = student:New()

--为对象中的字段赋值
stu.name = 'ann'
stu.age = 18
stu.number = 0001
 
--调用对象中的字段
print(stu.name)
print(stu.age)
print(stu.number)
 
--调用对象中方法，调用方法时
stu:info()

--3.代码分离
--面向对象的编程需要实现类与对象的分离。定义好一个类后，要实例化为对象时，需要引入其所在空间。

--Lua引用路径
--关键方法： 
dofile("路径+脚本名.lua") 
--如果当前脚本和要加载的脚本处于同一个文件夹中，只需要写脚本名即可。

--返回上一级目录进入class目录找到Person.lua文件
dofile("..\\class\student.lua")
 
--可直接调用student.lua中公开的字段和方法
cstudent = student:New("anna", 20, 0002)  --增加了新的构造方法，可给初始字段赋值
cstudent:info()
```
