---
title: Python
date: "2018-05-6 16:11:16"
tags: "Python"
categories: "Python"
---

环境:

- CentOS7x86_64
- Python3.5


参考:

- Python教程: <https://docs.python.org/3.5/tutorial/index.html>
- Python术语: <https://docs.python.org/3.5/glossary.html>
- Python语言参考: <https://docs.python.org/3.5/reference/index.html>
- Python HOWTOs: <https://docs.python.org/3.5/howto/index.html>
- Python标准库: <https://docs.python.org/3.5/library/>
- PyPI: <https://pypi.org/>
- Awesome-Python
	+ <https://github.com/vinta/awesome-python>
	+ <https://github.com/jobbole/awesome-python-cn>



<br>
<br/>

<!--more-->

---

<br>



# 术语

[Glossary](https://docs.python.org/3.6/glossary.html)


- `>>>`
交互式shell的默认Python提示符

- `...`
在为缩进代码块输入代码时，或在一对匹配的左右分隔符中，交互式shell的默认Python提示符

- `2to3`
将Python2.x代码转换为Python3.x代码的工具

- 抽象基类(abstract base class)

- 参数(argument)
调用函数时传递给(或方法)的值: 关键字参数/可选参数

- 异步上下文管理器(asynchronous context manager)
控制在异步语句中看到的环境对象

- 异步生成器(asynchronous generator)
返回一个生成器迭代器的函数

- 异步生成器迭代器(asynchronous generator iterator)
由异步生成器创建的对象

- 异步可迭代(asynchronous iterable)
一个对象

- 异步迭代器(asynchronous iterator)
一个对象

- 属性(attribute)
按名称引用的对象关联的值

- awaitable
一个对象

- 二进制文件(binary file)
能读写bytes-like对象的文件对象

- bytes-like对象
支持Buffer Protocol并可以导出C-contiguous buffer的对象

- 字节码(bytecode)
Python源代码被编译成字节码

- 类(class)
用于创建用户对象的模板

- coercion
在涉及两个相同类型参数的操作中，将一个类型的实例隐式转换为另一个类型的实例

- 复数(complex number)

- 上下文管理器(context manager)

- contiguous

- 协程(coroutine)

- coroutine function

- CPython
Python语言的规范实现

- 修饰器(decorator)
返回另一个函数的函数

- 描述(descriptor)

- 字典(dictionary)

- 字典视图(dictionary view)
从`dict.keys()`, `dict.values()`, `dict.items()`返回的对象称为字典视图

- 文档字符串(docstring)
在类，函数，或模块中的第一个表达式出现的字符串文字

- duck-typing
一种编程风格

- 表达式(expression)

- 扩展模块(extension module)
由C/C++编写，通过Python API与核心和用户代码交互

- f-string

- 文件对象(file object)

- finder
为正在导入的模块查找加载程序的对象

- 地板除(floor division)

- 函数(function)

- 函数注释(function annotation)

- `__future__`
可使用伪模块来启用与当前解释器不兼容的新语言功能

- 垃圾回收(garbage collection)
不再使用时释放内存的过程

- 生成器(generator)

- generator iterator

- 生成器表达式(generator expression)
返回迭代器的表达式

- 通用函数(generic function)
由多个函数组成的函数

- global interpreter lock
确保一次只有一个线程执行Python字节码的机制

- hashable
如果一个对象具有在其生命周期内从不改变的hash值，并且可与其它对象相比，那么这个对象就是可hash的

- IDLE
Python的集成开发环境

- 一成不变的(immutable)
具有固定值的对象

- 易变的(mutable)
可改变它们值得对象

- import path

- importing
一个模块中的Python代码在另一个Python代码中可获取

- importer
既能找到又能加载模块的对象

- 交互式(interactive)

- 解释型(interpreted)
Python是一种解释型语言，与编译型语言相反

- interpreter shutdown

- 迭代(iterable)
一次能够返回其成员的对象

- 迭代器(iterator)
表示数据流的对象

- 关键函数(key function)
关键函数或整理函数是一个可调用函数，它返回用于排序的值

- 关键字参数(keyword argument)

- lambda
一个匿名内联函数，由调用该函数时评估的单个表达式组成

- LBYL
三思而后行(Look before you leap)

- 列表(list)
一个内建Python序列

- list comprehension
一种紧凑的方式来处理序列中的全部或部分元素，并返回列表和结果

- loader
加载模块的对象

- 映射(mapping)
支持任意键查找并实现映射中指定方法的容器对象

- meta path finder

- metaclass
The class of a class

- 方法(method)
类里面定义的函数

- method resolution order

- 模块(module)
Python代码的组织单元的对象

- module spec

- named tuple

- 命名空间(namespace)
变量存储的地方

- namespace package
仅用作子包的包

- 嵌套范围(nested scope)
能够在封闭变量中引用变量

- new-style class

- 对象(object)
具有状态和定义行为的任一数据

- 包(package)
可包含子模块或递归子模块的Python模块

- 参数(parameter)
函数或方法定义中的一个命名实体，用于指定该函数可接受的参数。
有5中参数:
	+ positional-or-keyword:
	+ positional-only
	+ keyword-only
	+ var-positional
	+ var-keyword

- path entry

- path entry finder

- path entry hook

- path based finder

- path-like object

- portion
单目录中的一组文件

- positional argument

- provisional API

- provisional package

- Python 3000
Python3.x发行版的昵称

- Python化(Pythonic)
与Python语言最常见的习惯用法密切相关的想法或代码片段，而不是使用其它语言通用的概念来实现该代码

- 合格的名字(qualified name)

- 引用计数(reference count)
对某个对象的引用次数

- regular package

- `__slots__`
类中的声明，通过预先声明实例属性的空间并消除实例字典来节省内存

- 序列(sequence)

- 单一调度(single dispatch)
通用函数调度的一种形式

- 切片(slice)
通常包含一部分序列的对象

- special method
一种由Python隐式调用的方法

- 声明(statement)

- struct sequence
具有命名元素的元组

- text encoding

- text file

- 三重引号(triple-quoted string)

- type

- 通用换行符(universal newlines)
Unix: `\n`; Windows: `\r\n`

- 变量注释(variable annotation)
与模块全局变量或类属性关联的类型元数据值

- 虚拟环境(virtual environment)

- 虚拟机(virtual machine)

- Zen of Python
Pythono的设计原理和哲学






<br>
<br/>

---

<br>



# 教程

官网: <https://www.python.org/>

Python教程非正式地向读者介绍了Python语言和系统的基本概念和功能。
Python是一种易于学习，功能强大的编程语言。它具有高效的高级数据结构以及面向对象(object-oriented)编程的简单而有效的方法。优雅的语法和动态类型以及其解释的特性，使其成为大多数平台上许多领域脚本编写(scripting)和快速应用程序开发的理想语言。
Python解释器很容易用C或C++实现新功能和数据类型进行扩展。Python也适合作为定制程序的扩展语言。

本教程非正式地向读者介绍了Python语言和系统的基本概念和功能，不会涵盖每个功能。相反，它引入了许多Python最值得注意的功能和语言风格。


<br>
<br/>


## 激起你的胃口

Whetting Your Appetite

<br>

将一些工作自动化，或编写一个小程序。

C/C++/Java，编写/编译/测试/重编译周期太慢，但你又不想为你的应用程序开发和设计一门全新的语言。

这样的话，Python就是适合你的语言！

<br>

为一些任务编写Unix shell script或Windows batch file，但它们只适合文本数据，而不适合GUI应用程序...
你可以编写C/C++/Java程序，但需要很长的开发时间。Python简单易用，可帮助你更快完成工作。


Python为大型程序提供更多的结构和支持，它提供了更多的错误检查。作为一种非常高级的语言，它有内建的高级数据类型(如灵活的数组和字典)。
由于其更通用的数据类型，Python适用于比`awk`甚至`Perl`更大的问题域，但Python中的许多事情至少与这些语言一样容易。

Python允许你将你的程序拆分成模块，使其它Python程序能重用。它附带了大量的标准模块，你可将它们作为学习Python编程的基础。包括了: 文件I/O；系统调用；socket；GUI...

Python是一种解释型语言，在程序开发中节省大量时间，因为不需要编译和链接。

Python可以使程序紧凑而易读，由Python编写的程序通常比等效的C/C++/Java程序代码少得多。原因如下:

- 高级数据类型允许你在单个语句中表达复杂的操作
- 语句分组通过缩进(4个空格)来完成，而不是开始和结束
- 无需声明变量和参数

Python是可扩展的，如果你会C编程的话，很容易为解释器添加一个新的内置函数或模块，或将Python程序链接到可用库的二进制形式。也可将Python解释器链接到C编写的应用程序中。

顺便说一句，该语言是根据BBC节目**Monty Python’s Flying Circus**命名，与爬行动物无关。

学习语言的最好方法就是使用它，以工代练！


<br>
<br/>
<br/>


## 解释器

Python Interpreter

<br>

My Linux:

- `/usr/bin/python3`
- `/lib64/python3.5/`


<br>
<br/>


### 交互模式

Interactive Mode

```
python3

>>>for i in range(4):
...

```


<br>
<br/>


### 参数传递

Argument Passing

<br>

使用`sys`模块的`argv`变量给脚本传递参数。

```py
import sys


num = len(sys.argv)

if num != 3:
    print('Usage: xxx.py argv1 argv2')
else:
    print('argu[0] is ' + sys.argv[0])
    print('argu[1] is ' + sys.argv[1])
    print('argu[2] is ' + sys.argv[2])


chmod u+x xxx.py

./argvPass.py 1 22
argu[0] is ./argvPass.py
argu[1] is 1
argu[2] is 22
```


<br>
<br/>


### 编码格式

Source Code Encoding

```py
#!/usr/bin/python3
# -*- coding: utf-8 -*-

```


<br>
<br/>
<br/>


## 介绍

An Informal Introduction to Python

<br>

注意Python的两个默认提示符:

- `>>>`
- `...`


<br>
<br/>


### 作为计算器

Using Python as a Calculator

<br>

#### Numbers

```py
+
 -
*
/
//(取商)
%(取余)
**

int
float
decimal
fraction(分数)
comlex number(复数)
```

<br>

#### Strings

```py
'(single quote)
"(double quote)
\(转义)
r(元字符)
'''
"""

+
*

index
string[-1]
string[0:2]

len()
```

<br>

#### Lists

```
list = [xx, x, ...]

index
list[index]
list[start:stop]

method
append()
pop()
del()
...
```


<br>
<br/>


### 编程第一步

First Steps Towards Programming

<br>

斐波那契数列(Fibonacci series)

```py
a, b = 0, 1
while b < 10:
    print(b, end=',')
	a, b = b, a+b

 #多重赋值(multiple assignment)
 #关键字参数(keyword argument)
```


<br>
<br/>
<br/>


## 控制流

Control Flow Tools

<br>

### while

while Statements

```py
a = input(int('Please input an int: '))

whiel a < 50:
	a += 1

print(a)
```

<br>

### if

if Statements

```py
x = int(input("please input an int: "))

if x < 0:
    x = 0
    print('Negative changed to zero')
elif x == 0:
    print('Zero')
elif x == 1:
    print('Single')
else:
    print('More')
```

<br>

### for

for Statements

```py
words = ['a', 'bb', 'ccc']

for w in words:
	print(w, len(w))


'''
如果需要修改迭代中的序列，建议先制作副本，遍历一个序列并不会隐式地创建一个副本
'''
words = ['a', 22, 'ccc']
for w in words[:]:
    if type(w) is int:
		words.insert(0, w)

words
[22, 'a', 22, 'ccc']
```

<br>


### range

The range() Function
遍历一系列数字

```py
for i in range(5):
	print(i)


for i in range(0, 101, 10):
	print(i)


a = [1, 22, 'A', 'AA']
for i in range(len(a))
	print(i, a[i])

list(range(5))
[0, 1, 2, 3, 4]
```

<br>

**注意**
在许多方面，由`range()`返回的对象的行为就好像它是一个列表，但事实并非如此。它是一个对象，在你迭代时才返回所需序列，但它并不真正生成列表，从而节省空间。
我们说这样一个对象是可迭代的(iterable)。

```py
print(range(10))
range(0, 10)

```

<br>

### break/continue

break and continue Statements, and else Clauses on Loops

- `break`
	+ 结束循环
- `continue`
	+ 结束本次循环

<br>

### pass

pass Statements
`pass`语句什么也不做！当语句需要语法而程序不需要任何操作时，可使用它。

```py
while True:
    pass

class emptyClass:
    pass
```

<br>

### 函数定义

Defining Functions
关键字`def`引入一个函数定义，必须跟随函数名称和形式参数。函数主体语句必须缩进

函数主体的第一个语句是可选的字符串文字(sting literal)，用于描述函数。
在编写的代码中包含文档字符串是一种很好的做法，请养成此习惯。

函数中的所有变量赋值都将值存储在本地符号表中，而变量引用首先在本地符号表中查找，然后是封闭函数的本地符号表，然后是全局符号表，最后是内置名称表。
因此，全局变量不能直接在函数内赋值(除非是`global`语句)，尽管它们可能被引用。

事实上即使是没有`return`语句的函数也会返回一个值，它被称为`None`(一个内建名)

```py
def fib(n):
	"""function's documentation
	   print a Fibonacci series up to n.
	"""
    a, b = 0, 1
	while a < n:
		print(a, end='  ')
		a, b = b, a+b
	print()

fib(100)

f = fib
f(100)

print(fib())
None
```

<br>

也可以使用可变数量的参数来定会函数。

<br>

#### 默认参数值

Default Argument Values

最有用的形式是为一个或多个参数指定默认值。

```py
def ask_ok(prompt, retries=4, reminder='Please try again!'):
    while True:
        ok = input(prompt)
        if ok in ('y', 'ye', 'yes'):
            return True
        if ok in ('n', 'no', 'nop', 'nope'):
            return False
        retries = retries - 1
        if retries < 0:
            raise ValueError('invalid user response')
        print(reminder)

```

函数可通过如下方法调用:

- 只给出必须的参数: `ask_os('Prompt xxx')`
- 给出可选参数: `ask_ok('Prompt xx', 3)`
- 给出所有参数: `ask_ok(agr1, arg2, arg3)`

<br>

#### 关键字参数

Keyword Arguments

也可使用`kwarg = value`来调用函数。

```py
def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
    print("-- This parrot wouldn't", action, end=' ')
    print("if you put", voltage, "volts through it.")
    print("-- Lovely plumage, the", type)
    print("-- It's", state, "!")



parrot(1000)                                          # 1 positional argument
parrot(voltage=1000)                                  # 1 keyword argument
parrot(voltage=1000000, action='VOOOOOM')             # 2 keyword arguments
parrot(action='VOOOOOM', voltage=1000000)             # 2 keyword arguments
parrot('a million', 'bereft of life', 'jump')         # 3 positional arguments
parrot('a thousand', state='pushing up the daisies')  # 1 positional, 1 keyword
```

<br>

#### `*name/**name`

`**name`，它接收一个字典(`keyword=value`)。可能与`*name`结合使用。`*name`必须出现在`**name`之前。

```py
def shop(kind, *arguments, **keywords):
    print("-- Do you have any ", kind, "?")
    print("-- I'm sorry, we're all out of ", kind)
    for arg in arguments:
        print(arg)
    print('\n-----\n')
    for kw in keywords:
        print(kw, ':', keywords[kw])


shop('Kind', 'arg1', 'arg2', kw1='KW1', kw2='KW2', kw3='KW3')

"""
-- Do you have any  Kind ?
-- I'm sorry, we're all out of  Kind
arg1
arg2

-----

kw3 : KW3
kw1 : KW1
kw2 : KW2
"""
```

<br>

#### 任意参数列表

Arbitrary Argument Lists

最不经常使用的选项是指定可以用任意数量的参数调用一个函数，这些参数将被封装在一个元组中。

```py
def arb(*args):
	for arg in args:
		print(arg)

art(1, 22, 'CCC')
```

<br>

#### 解包参数

Unpacking Argument Lists

当参数已经在一个列表或元组中时，会出现相反的情况。需要对单独的位置参数的函数调用进行解包。

```py
list(range(5))
[0, 1, 2, 3, 4]

args = [5]
list(range(*args))
[0, 1, 2, 3, 4]

```

<br>

#### Lambda表达式

可以使用`lambda`关键字创建小的匿名函数。
Lambda函数可用于需要函数对象的任何地方，它在语法上受限于单个表达式。

```py
def lambdaTest(n):
	return lambda x: x + n

f = lambdaTest(10)
f(1)
11

f(5)
15
```

<br>

#### 文档字符串

Documentation Strings

以下是关于文档字符串内容和格式的一些约定:

- 第一行应该始终是对象目的的简短摘要
- 第二行应该是空白，如果有多行的话
- 以下几行应该是描述

```py
def func():
    """Document it.

	This func just print one argument.
	"""

	print(sys.argv[1])


print(func.__doc__)

Document it.
    This func just print one argument.
```

<br>

#### 函数注释

Function Annotations

函数注释完全是关于用户定义函数使用的类型的可选元数据信息。

Annotations以字典的形式存储在函数的`__annotations__`属性中，并且不影响函数的其它部分。参数注释由参数名称后面的冒号`:`定义，后跟表达式评估注释的值。注释由参数列表和`def`语句结束的冒号之间的`->`定义，后跟一个表达式。

```py
def f(name: str, age: int = 18) -> str:
    print("Annotations: ", f.__annotations__)
    print("Arguments: ", name, age)
    return name + 'and' + str(age)

f('Zhang21')
```

<br>

### 编码风格

不同的语言有不同的编码风格。但是，让别人很轻松便能阅读你的代码总是一个好主意！

对于Python而言，PEP(Python Enhanced Proposals) 8 已成为大多数项目遵循的风格指南。它促进了非常可读和令人喜爱的编码风格，每个Python开发者都应该阅读它。
以下是最重要的几点：

- 使用`4空格`缩进，而不是`tab`
- `自动换行`，不要超过79个字符
- 使用`空白行`来分割函数和类，以及函数内的更大快代码
- 如有可能，请将`注释`放在它们的上一行
- 使用`文档字符串`
- 在运算符和逗号后面使用空格，但不要直接在包围结构中使用空格 -> `(a + b)`
- 一致地命名函数和类
- 建议使用`UTF-8`编码方式
- 建议不要在标识符中使用non-ASCII字符，如果有其它语言的人会去维护代码



<br>
<br/>
<br/>



## 数据结构


Data Structures

<br>

### 列表

More on Lists

<br>

列表数据类型有多种方法：

- `list.append(x)`
添加一个项到列表的末尾

- `list.extend(iterable)`
通过添加迭代中的所有项来扩展列表

- `list.insert(i, x)`
在列表中给定位置插入一个项

- `list.remove(x)`
删除列表中给定值的第一项

- `list.pop()`
返回并删除列表中给定位置的项
如果未指定index，则默认为最后一项

- `list.clear()`
删除列表中的所有项

- `list.index(x)`
返回指定值的第一个索引
如果没有此值，返回ValueError

- `list.count(x)`
返回列表中指定值出现的次数

- `list.sort()`
对列表中的项进行排序

- `list.reverse()`
反转列表中的元素

- `list.copy()`
返回列表的shallow copy


<br>

**列表用处：**

- Stack

- Queue


- 列表解析
List Comprehensions

```py
 #列表解析提供了一个简洁的方式来创建列表
l = []
for i in range(10):
    l.append(i**2)

 #lambda
l = list(map(lambda i: i**2, range(10)))

 #or
l = [x**2 for i in range(10)]


[(x, y) for x in [1, 2, 3] for y in [3, 2, 1] if x != y]
```

- 嵌套列表解析
Nested List Comprehensions

列表解析中的初始表达式可以是任意表达式，包括另一个列表解析。

```py
l = [
    [1, 2, 3, 4],
	[5, 6, 7, 8],
	[9, 10 ,11, 12]
]

[[row[i] for row in l] for i in range(4)]
```


<br>
<br/>


### del语句

`del`语句可从列表中删除切片或整个列表。

```py
a = [1, 2, 3, 4]

del a[0]

del a[1:3]

del a[:]

del a
```


<br>
<br/>


### 元组和序列

Tuples and Sequences

<br>

列表和字符串由许多共同属性，如索引和切片操作。
列表是可变的，它们的元素通常是同类，并且通过遍历列表可访问。

元组是不可变的！无法对元组项赋值，但可创建包含可变对象的元组。

```py
t = (123, 321, 'hello')

tt = t, ('a', 'bb')

(123, 321, 'hello), ('a', 'bb')

 #对元组赋值会出错
t[0] = 888
TypeError: 'tuple' object does not support item assignment


 #序列拆包(unpacking)
 #要求变量数量等于元素数量
x, y, z = t

x
123

y
321

z
'hello'
```


<br>
<br/>


### 集合

Sets

<br>

Python中的集合是没有重复元素的无序集合，并支持数学操作:

- 并集 `a | b`
- 交集 `a & b`
- 差集 `a - b`
- 异或 `a ^ b`

使用大(花)括号`{}`或`set()`创建集合，但创建一个空集合使用`set()`，而不是`{}`——后者创建一个空字典。

集合也支持集合解析。

```py
alpha = {'a', 'b', 'c', 'a'}

alpha
{'c', 'b', 'a'}

'c' in alpha
True


 #数学运算
a = set('abracadabra')
b = set('alacazam')

a | b
a & b
a - b
a ^ b


 #集合解析
{x for x in 'abcdefgabc' if x not in 'abc'}
```


<br>
<br/>


### 字典

Dictionaries

<br>

字典数据类型在其它语言中被称为“associative memories” or “associative arrays”。与由数字索引的序列不同，字典由`key`索引(可以是任何不可变类型)，字符串和数字都可作为`key`。如果元组只包含字符串，数字或元组，则可作为`key`。若包含任何可变对象，则不能作为`key`。你不能使用列表作为`key`。

可将字典视为无序的键:值对，并要求键是唯一！
花括号`{}`创建一个空字典。

字典的主要操作是用某个`key`存储`value`，并提取给定`key`的`value`。使用`del`语句删除一个键值对；新键值对会替换旧键值对。

```py
info = {'name': 'AA', 'id': 1, 'tel': 155}
info['addr'] = 'Chengdu'

info
info['name']

del info['id']

list(info.keys())
print(info.values())
print(info.items())


 #dict()构造函数
dict([('name', 'A'), ('age', 11)])
dict({'name': 'A', 'age': 11})
dict(name='zhang', age=11)

 #字典解析
{x: x**2 for x in (2, 4, 6)}
```


<br>
<br/>


### 循环技巧

Looping Techniques

<br>

- 字典循环

```py
info = {'name': 'AA', 'age': 11}
for k, v in info.items():
    print(k, v, sep=':')

name:AA
age:11
```

- 序列循环
可使用`enumerate()`函数同时检索位置索引和相应值

```py
for i, v in enumerate(['a', 'b', 'c']):
    print(i, v)

0 a
1 b
2 c
```

- 同时循环多个序列
要同时循环多个序列，可将这些条目与`zip()`函数配对

```py
aa = [1, 2, 3]
bb = ['a', 'b', 'c']

for a, b in zip(aa, bb):
    print('{0}, {1}'.format(a, b))

1, a
2, b
3, c
```

- 反向循环序列

```py
for i in reversed(range(6)):
    print(i, end=',')

5,4,3,2,1,0,
```

- 循环排序

```py
l = ['ac', 'fb', 'nx', 'by']
for i in sorted(l):
    print(i)

ac
by
fb
nx


ll = ['ac', 'fb', 'nx', 'by', 'ac', 'by']
for i in sorted(set(ll)):
    print(i)

ac
by
fb
nx
```


<br>
<br/>


### 关于条件

More on Conditions

<br>

`while`和`if`语句中使用的条件可以包含任何运算符，而不仅仅是比较。

比较操作符`in`和`not in`检查值是否在序列中
操作符`is`和`is not`比较两个对象是否相同，这适用于可变对象(如list)

比较操作可以使用布尔运算符`and`和`or`进行组合，结果可用`not`。它们的优先级低于比较操作
所有的比较操作符(comparison operators)具有相同的优先级，都低于数值运算符


<br/>
<br>
<br>



## 模块


Modules

<br>

如果你从Python解释器中退出并重新进入，你所做的定义(函数和变量)将会丢失。因此，如果编写一个稍长的程序，最好使用文本编辑器，然后将代码文件作文输入来运行它。这就被称为创建一个脚本。
随着程序变长，可能需要将其分割为便于维护的多个文件。你可能还想使用你在多个程序中编写的某个功能(函数)，而不是将其定义复制到每个程序中。

为了支持此，Python有一种方法可将定义(definition)放入一个文件中，并在脚本或交互式实例中使用它们。这样的文件被称为模块(module)。
来自模块的定义可以被导入到其它模块或主模块中。

模块是一个包含Python定义和语句的文件。文件名是带有`.py`的模块名。
在模块中，模块的名称(string)用作全局变量`__name__`的值。

<br>

**编写一个模块：**
`vim /path/fibo.py`

```py
 #Fibonacci numbers module

def fib(n):
    a, b = 0, 1
	while b < n:
	    print(b, end=' , ')
		a, b = b, a+b
	print()

def fib2(n):
	result = []
	a, b = 0, 1
	while b < n:
		result.appen(b)
		a, b = b, a+b
	return result
```

**载入此模块：**
如果没有将此模块放入Python默认lib目录(如/usr/lib64/python3.5/)的话，则需要进入模块所在目录打开Python解释器。

```py
cd /path

python3

>>> import fibo
>>> fibo.fb(100)
1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89,
>>> fibo.fib2(100)
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
>>> fibo.__name__
'fibo'
>>> fibo.__str__()
module 'fibo' from '/path/fibo.py'
```

<br>

### 更多模块信息

More on Modules

一个模块可以包含可执行语句以及函数定义。这些语句旨在初始化模块，它们仅在`import`语句中第一次遇到模块名称时执行。

每个模块都有自己的私人符号表(private symbol table)，它被模块中定义的所有函数(functions)用作全局符号表(global symbol talbe)。因此，模块的作者可以在模块中使用全局变量(global variable)，而不用担心与用户的全局变量发生意外冲突。

模块可以导入其它模块。习惯上(但不是硬性要求)，将`import`语句放在模块(脚本)的开头。导入模块的名称被放置在导入模块的全局符号表中。

<br>

- 将模块名称直接导入到导入模块的符号表中，这不会在本地符号表中引入导入模块的名称

```py
 #fibo模块名并没有被定义
from fibo import fib, fib2
```

- 导入模块中定义的所有名称
在大多数情况下，Python程序员不会使用这个工具。因为它会向解释器引入一组未知的名称，可能会隐藏你已经定义的一些东西。
注意，通常从模块或包中`import *`的做法是不被接受的，因为它经常会导致代码可读性很差。但是，可以使用它来保存交互式会话中的输入。

```py
 #这会导入除了以下划线开头的所有名称
from fibo import *
```

- 将导入模块名称绑定到指定名称

```py
import fibo as fib

fib.fib(100)
fib.fib2(100)


from fibo import fib2 as fibonacci

fibonacci(100)
```

<br>

#### 把模块作为脚本来执行

Executing modules as scripts

如果你将模块中的`__name__`设置为`__main__`，模块中的代码就会被执行，就像导入它一样。这意味着你需要在你的模块的末尾添加它们。
如果模块被导入，代码也不会执行。

这通常用于为模块提供用户接口，或测试。

<br>

```py
if __name__ == "__main__":
    import sys
	fib(int(sys.argv[1]))


python3 fibo.py {args}
```

<br>

#### 模块的搜索路径

The Module Search Path

当`import fibo`模块时，解释器首先在内建模块中搜索此名称。如果找不到，它会在`sys.path`给出的目录列表中搜索`fibo.py`文件。

`sys.path`从以下位置初始化：

- 包含输入脚本的目录(未指定文件时的当前目录)
- PYTHONPATH
- 依赖于安装的默认值
- 包含符号链接的目录不会被添加到模块的搜索路径中

<br>


#### 编译的Python文件

Compiled Python files

为了加速载入模块，Python将每个模块的编译版本缓存在名为`module.version.pyc`的`__pycache__`目录下，对编译文件的格式进行编码，它通常包含Python版本号。
Python根据编译后的版本检查源代码的修改日期，看它是否过期并需要重新编译。这是一个完全自动的过程。另外，编译后的模块时独立于平台的，因此可以在不同体系结构的系统之间共享相同的库。

有两种情况，Python不会检查缓存：

- 总是重新编译并且不存储从命令行直接加载的模块的结果
- 没有源模块

<br>

**专家提示：**

- 可以Python命令中使用`-0`或`-00`来减少已编译模块的大小
- 读取`.pyc`文件不会比`.py`文件快，唯一更快的事情是它们被加载的速度
- 模块`compileall`可以为目录中的所有模块创建`.pyc`文件
- 更多细节，参见PEP 3147



<br>
<br/>


### 标准模块

Standard Modules

<br>

Python提供了一个标准模块库。

一些模块被内置到解释器中，提供了对操作的访问。这些操作不属于语言核心的一部分，但是为了提高效率或提供对操作系统的访问权限。

```py
import sys

sys.ps1
'>>>'
sys.ps2
'...'

sys.ps1 = '<<<'
sys.ps1
'<<<'


 #查看PYTHONPATH
sys.path.__str__
 #添加PYTHONPATH
sys.path.append('/home/zhang/venv/python')
```


<br>
<br/>


### `dir()`函数

内建函数`dir()`用于找出模块定义的名称。
它列出所有类型的名称： 变量，模块，函数...

```py
import fibo, sys

dir(fibo)
['__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'fib', 'fib2']


dir(sys)
['__displayhook__', '__doc__', '__excepthook__', '__interactivehook__', '__loader__', '__name__', '__package__', '__spec__', '__stderr__', '__stdin__', '__stdout__', '_clear_type_cache', '_current_frames', '_debugmallocstats', '_getframe', '_home', '_mercurial', '_xoptions', 'abiflags', 'api_version', 'argv', 'base_exec_prefix', 'base_prefix', 'builtin_module_names', 'byteorder', 'call_tracing', 'callstats', 'copyright', 'displayhook', 'dont_write_bytecode', 'exc_info', 'excepthook', 'exec_prefix', 'executable', 'exit', 'flags', 'float_info', 'float_repr_style', 'getallocatedblocks', 'getcheckinterval', 'getdefaultencoding', 'getdlopenflags', 'getfilesystemencoding', 'getprofile', 'getrecursionlimit', 'getrefcount', 'getsizeof', 'getswitchinterval', 'gettrace', 'hash_info', 'hexversion', 'implementation', 'int_info', 'intern', 'last_traceback', 'last_type', 'last_value', 'maxsize', 'maxunicode', 'meta_path', 'modules', 'path', 'path_hooks', 'path_importer_cache', 'platform', 'prefix', 'ps1', 'ps2', 'setcheckinterval', 'setdlopenflags', 'setprofile', 'setrecursionlimit', 'setswitchinterval', 'settrace', 'stderr', 'stdin', 'stdout', 'thread_info', 'version', 'version_info', 'warnoptions']


 #它不会列出内建函数和变量的名称，除非如下操作
import builtins
dir(builtins)

['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException', 'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning', 'ChildProcessError', 'ConnectionAbortedError', 'ConnectionError', 'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning', 'EOFError', 'Ellipsis', 'EnvironmentError', 'Exception', 'False', 'FileExistsError', 'FileNotFoundError', 'FloatingPointError', 'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError', 'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError', 'IsADirectoryError', 'KeyError', 'KeyboardInterrupt', 'LookupError', 'MemoryError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented', 'NotImplementedError', 'OSError', 'OverflowError', 'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError', 'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning', 'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError', 'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError', 'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeError', 'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning', 'ValueError', 'Warning', 'ZeroDivisionError', '_', '__build_class__', '__debug__', '__doc__', '__import__', '__loader__', '__name__', '__package__', '__spec__', 'abs', 'all', 'any', 'ascii', 'bin', 'bool', 'bytearray', 'bytes', 'callable', 'chr', 'classmethod', 'compile', 'complex', 'copyright', 'credits', 'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'exit', 'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr', 'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass', 'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview', 'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property', 'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice', 'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars', 'zip']
```


<br>
<br/>


### 包

Packages

<br>

包是通过”dotted(.) module names“，来构造Python模块命名空间的一种方式。

假设你想设计一个模块集(包)来统一处理声音文件和声音数据。有许多不同的声音文件格式。因此你需要创建和维护不断增长的模块集合，以便在各种文件格式之间进行转换。你还可能需要对声音数据执行各种不同的操作，因此你还需要编写无止境的模块流以执行这些操作。

这可能是一个包结构:

```
sound/                          Top-level package
      __init__.py               Initialize the sound package
      formats/                  Subpackage for file format conversions
              __init__.py
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  Subpackage for sound effects
              __init__.py
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  Subpackage for filters
              __init__.py
              equalizer.py
              vocoder.py
              karaoke.py
              ...
```

当导入包时，Python将搜索`sys.path`，并查找包的子目录。
需要`__init__.py`文件才能使Python将目录视为包含包。这是为了防止具有通用名称的目录(如字符串)无意中隐藏稍后在模块搜索路径中发生的有效模块。
在最简单的情况下，`__init__.py`可以是一个空文件，但它也可以执行包的初始化代码。

导入包:

- `from package import item`
 item可以是子模块，子包，函数，类，变量
 import语句首先测试项目是否在包中定义。 如果不在，它假定它是一个模块并尝试加载它。如果找不到它，则会引发ImportError异常

- `import item.subitem.subsubitem`
相反，当使用这种语法时，除最后一项外必须都是一个包，最后一项可以是模块或包，但不能是类或函数或变量




```py
import sound.effects.echo

from sound.effects import echo

```

<br>

#### importing * from a package

当输入`from sound.effects import *`会发生什么？理想情况下，人们会希望以某种方式进入文件系统，查找包中存在哪些子模块，然后将它们全部导入。这可能需要很长时间，并且导入子模块可能具有不希望的副作用，这些副作用在明确导入子模块时才会发生。

唯一的解决方案是软件包作者提供包的明确索引。`import`使用以下声明: 如果某个包的`__init__.py`定义了一个名为`__all__`的列表，则它将成为`from package import *`时应该导入的模块名称列表。
当软件包新版本发布时，软件包作者需要保持该列表是最新版本。

栗子`sound/effects/__init__.py`:

```
 #这意味着from sound.effects import *只会导入以下子模块
__all__ = ["echo", "surround", "reverse"]
```

<br>

如果`__all__`没有被定义，则`from sound.effcts import *`语句不会将包`sound.effects`中所有子模块导入到当前命名空间。它只能确保包`sound.effects`被导入，然后导入包中定义的任何名称。这包括`__init__.py`定义的任何名称，还包括由以前的导入语句显示加载的软件包的任何子模块。

请记住，使用`from packagee import submodule`没有任何问题。事实上，这也是推荐的方法。除非导入模块需要使用不同包中具有相同名称的子模块。

<br>

#### 内部包装

Intra-package References

当包被构建为子包时，可以使用绝对导入来引用邻包中的模块。
同样，也可以使用相对导入来导入邻包中的模块。

```
 #Absolute
from sound.effects import echo


 #Relative
from . import echo
from .. import formats
from ..filter import equalizer
```


<br>


#### 多个目录中的包

Packages in Multiple Directories

包还支持一个特殊的属性`__path__`。在执行该文件中的代码之前，它被初始化为一个包含`__init__.py`的目录名称的列表。这个变量可以修改，这样做会影响将对包中包含的模块和子包的搜索。

虽然此功能通常不是必需的，但它可用于扩展包中找到的一组模块。



<br>
<br/>

---

<br/>



## 输入和输出


Input and Output
有多种方式来呈现程序的输出；数据也可以打印成人类可读的形式，或写入文件供将来使用。

<br>


### 幻想的输出格式

Fancier Output Formatting

到目前为止，我们知晓两种写入值的方法:

- 表达式语句
- `print()`函数

有两种方法可以格式化输出：

- 自己完成所有的字符串处理(使用切片和连接操作，你可创建任何你能想到的布局)
- 格式化字符串文字或`str.format()`方法

<br>

string模块提供了一个Template类，它提供了另一种将值替换为字符串的方法。
Python有办法将任何值转换为字符串：将它传递给`repr()`或`str()`函数。

`str()`函数，用于返回相当可读(human-readable)的值的表示
`repr()`函数，用于生成可由解释器读取的表示
对于没有人定义的特定表示的对象，`str()`将返回与`repr()`相同的值

```py
for x in range(1, 6):
    print(repr(x).rjust(2), repr(x**2).rjust(3), end=' ')
	print(repr(x**3).rjust(4))

 #Or
for x in range(1, 6):
    print('{0:2d} {1:3d} {2:4d}'.format(x, x**2, x**3))


  1   1    1
  2   4    8
  3   9   27
  4  16   64
  5  25  125
```

<br>

字符串对象的`str.rjust()方法`，它在给定宽度的字段中通过填充左边的空格来右对齐字符串。类似方法还有: `str.ljust()`, `str.center()`。这些方法不写入任何东西，它们只是返回一个新的字符串。

还有一种`str.zfill()`方法，它在数字字符串的左边填充数字0，它能识别加号和减号：

```py
>>> '12'.zfill(5)
'00012'
>>>
>>> '-3.14'.zfill(7)
'-003.14'
>>>
>>> '3.1415678'.zfill(5)
'3.1415678'
```

<br>

`str.format()`方法:

```py
 #{}
print('We are {} who say {} is "{}!".format('A', 'BB', 'WONDERFUL'))
We are A who say BB is "WONDERFUL!"


 #括号中的数字用来指向传入的位置
 #{index},从0开始
print('{0} and {1}'.format('A', 'BB'))
A and BB

print('{1} and {0}'.format('A', 'BB'))
BB and A


 #关键字参数
print('My name is {name}, I\'m {age} years old!'.format(name='Zhang21', age=21))
My name is Zhang21, I'm 21 years old!


 #位置参数和关键字参数的组合
print('The story of {0}, {1}, and {other}'.format('A', 'BB', other='CCC'))
The story of A, BB, and CCC


'''
!a, 应用ascii()
!s, 应用str()
!r, 应用repr()
:, 更好的控制格式
:5
:7d
:.3f
'''
print('My full name is {!s}'.format('Zhang21'))
My full name is Zhang21

print('My full name is {!r}'.format('Zhang21'))
My full name is 'Zhang21'

print('The value of {} is approximately {:.3f}'.format('PI', 3.141567))
The value of PI is approximately 3.142

info = {'A': 68, 'BB': 79, 'CCC': 89}
for k, v in info.items():
     print('{0:5} ==> {1:6d}'.format(k, v))

A     ==>     68
BB    ==>     79
CCC   ==>     89

print('A: {A:d}; B: {BB:d}, C: {CCC:d}'.format(**info))
A: 68; B: 79, C: 89
```

<br>


`%`操作符同样可用于字符格式化:

```py
print('The value of %s is approximately %5.3f' % ('PI', 3.1415678))
The value of PI is approximately 3.142
```


<br>
<br/>


### 读写文件

Reading and Writing Files

`open()`返回一个文件对象，它最常用的两个参数：`open(filename, mode)`

```py
f = open('/tmp/1.txt, 'r')
f.readline()
'1\n'

f.closed
Fasle

f.close()
f.closed
True
```

mode:

- `r`: read only，未指定模式时的默认模式
- `w`: only writing
- `a`: appending
- `r+`: reading and writing
- `b`: binary mode

通常情况下，文件以文本模式打开，这意味着你可读写文件中的字符串，并以特定编码方式进行编码(如UTF-8)。如果未指定编码，则默认值取决于平台。
`b`以二进制模式打开文件，数据以字节对象的形式读写，该模式应该用于所有不包含文本的文件。在读写文件时要非常小心的使用二进制模式。

推荐使用`with`关键字处理文件对象，优点是，即使在某个时间点出现异常，文件在其套件结束后也能正常关闭。也比`try-finally`块短得多。
如果没有使用`with`关键字，则你需要调用`f.close()`来关闭文件，并立即释放它使用的系统资源。如果你没有明确关闭一个文件，Python的垃圾回收器最终会摧毁这个对象并为你关闭吧这个打开的文件，但这个文件可能会保持打开一段时间。
在关闭文件对象之后，尝试使用文件对象将会自动失败。

```py
with open('/tmp/1.txt') as f:
    read_Data = f.read()

f.closed
True


f.read()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: I/O operation on closed file.
```


<br>


#### 文件对象方法

Methods of File Objects

`f.read(size)`读取文件内容，以字符串或字节对象的形式返回。size是一个可选的数值参数，当size被忽略或为负数时，文件的全部内容被读取并返回。如果超过内存限制，那就是你的问题了。
`f.readline()`从文件读取一行，换行符Unix`\n`，Windows`\r\n`
`f.readlines()`, `list(f)`读取文件的所有行
`f.write(string)`向文件中写入字符内容，并返回写入的字符数
`f.tell()`返回一个整数，表示二进制模式下文件开头的字节数
`f.seek(offset, from_what)`改变文件对象的位置

```py
f = open('/tmp/1.txt', 'r+')
f.write('Line4\n')
6

string = ('AAA', 11)
s = str(string)
f.write(s)
11


f = open('/tmp/1.txt', 'rb+')
f.write(b'0123456789abcde')

f.seek(1)
1
f.read(1)
b'1'
```


<br>


#### 使用json保存结构化数据

Saving structured data with json

字符串可以很容易地读写文件和从文件读取。当你想要保存更复杂的数据类型——如嵌套列表和字典，手动解析和序列化将变得很复杂。

JSON格式通常被现代应用程序用于数据交换。
Python允许你使用名为JSON的流行数据交换格式。称为json的标准模块可采用Python数据层次结构，并将其转换为字符串表示形式，这个过程被称为序列化(serializing)。重建字符串表示中的数据称为反序列化(deserializing)。

```py
import json
json.dumps([1, 'simple', 'list'])
'[1, "simple", "list"]'

json.dump(x, f)
x = json.load(f)
```



<br>
<br/>
<br/>



## 错误和异常

Errors and Exceptions

至少有两种可区分的错误:

- syntax errors
- exceptions


<br>
<br/>


### 语法错误

Syntax Errors
语法错误，也称为解析错误。这是最常见的语法问题错误。


<br>
<br/>


### 异常

Exceptions
即使语法是正确的，但在执行时也可能导致错误。执行过程中检查到的错误称为异常。
Built-in Exceptions列出了内置的异常及其含义。

```py
 >>> 10 * (1/0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero

 >>> 4 + spam*3
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'spam' is not defined

 >>> '2' + 2
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: Can't convert 'int' object to str implicitly
```


<br>
<br/>


### 处理异常

Handling Exceptions
编写处理选定异常的程序是可能的。

```py
while True:
    try:
        x = int(input("Please enter a number: "))
        break
    except ValueError:
        print("Oops!  That was no valid number.  Try again...")


 #多个异常放入一个元组
except (RuntimeError, TypeError, NameError):
    pass
```

<br>

**`try`语句工作原理：**

- 首先，`try`子句(`try...except`之间的语句)被执行
- 如果没有异常发生，则执行`try`语句并跳过`except`子句后便结束
- 如果在执行`try`子句时发生异常，则跳过子句的其余部分。然后，如果**异常类型**匹配`execpt`后面的异常名称，则`except`子句被执行，然后在`try`语句后继续执行
- 如果产生的异常与`except`的异常名称不匹配，它将传递给外部`try`语句。如果没有找到处理程序，则它是一个未处理的异常，执行停止并显示错误消息
- `try`语句可能有多个`except`子句，用于处理不同的异常。最多只有一个处理程序被执行
- 处理程序只处理发生在相应`try`子句中的异常，而不处理相同`try`语句的其它处理程序
- `except`子句可将多个异常名放入一个元组
- 如果是相同的类或其基类，则`except`子句中的类与异常兼容
- 未使用异常名称的`except`子句作为通配符
	+ 请谨慎使用此功能，因为以这种方式很容易掩盖真正的编程错误
- `try...except`语句还有一个可选的`else`子句。当存在时，它必须遵循所有`except`子句。如果`try`子句不引发异常，则必须执行该代码

```py
import sys

try:
    f = open('/tmp/1.txt')
    s = f.readline()
    i = int(s.strip())
except OSError as err:
    print('OS error: {}'.format(err))
except ValueError:
    print("Could not convert data to an integer.")
except:
    print("Unexpected erros", sys.exc_info()[0])
    raise

```

```py
try:
    sum = 'a' + 1
except TypeError:
    print('TypeError')
else:
    print('else: ', sum)
```


<br>
<br/>


### 引发异常

Raising Exceptions

`raise`语句允许程序员强制执行指定的异常。

```py
>>> raise NameError('HiThere')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: HiThere



>>> try:
...     raise NameError('HiThere')
... except NameError:
...     print('An exception flew by!')
...     raise
...
An exception flew by!
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
NameError: HiThere
```


<br>
<br/>


### 用户定义的异常

User-defined Exceptions

程序可以通过创建一个新的异常类(exception class)来为自己的异常命名。异常通常应该直接或间接地从 Exception class 派生。

可以定义异常类，它可以执行任何其它类可以执行的任何操作，但通常很简单，通常只提供一些属性，以便处理程序为异常提取有关错误的信息。
创建可引发多个不同错误的模块时，通常的做法是为该模块定义的异常创建基类，并创建用于为不同错误条件创建特定异常类的子类:

```py
class Error(Exception):
    """Base class for exceptions in this module."""
    pass

class InputError(Error):
    """Exception raised for errors in the input.

    Attributes:
        expression -- input expression in which the error occurred
        message -- explanation of the error
    """

    def __init__(self, expression, message):
        self.expression = expression
        self.message = message

class TransitionError(Error):
    """Raised when an operation attempts a state transition that's not
    allowed.

    Attributes:
        previous -- state at beginning of transition
        next -- attempted new state
        message -- explanation of why the specific transition is not allowed
    """

    def __init__(self, previous, next, message):
        self.previous = previous
        self.next = next
        self.message = message
```

大多数异常的名称都以**Error**结尾来定义，类似于标准异常的命名。
许多标准模块定义了它们自己的异常，用于在其定义的功能中可能发生的错误。


<br>
<br/>


### 定义清理行为

Defining Clean-up Actions

try语句还有一个可选的子句，用于定义在任何情况下都必须执行的清理操作(clean-up actions).

finally子句总是在离开try语句之前执行，无论是否发生异常。

```py
>>> try:
...     raise KeyboardInterrupt
... finally:
...     print('Goodbye, world!')
...
Goodbye, world!
KeyboardInterrupt
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
```


<br>
<br/>


### 预定义的清理操作

Predefined Clean-up Actions

某些对象定义了在不再需要对象是要执行的标准清楚操作，而不管使用对象的操作是成功还是失败。

```py
with open("myfile.txt") as f:
    for line in f:
        print(line, end="")
```



<br>
<br/>
<br/>



## 类


Classes

类提供了将数据和功能捆绑在一起的手段。每个类实例都可附加属性以保持其状态。类实例也可以有方法来修改其状态。

与其它编程语言相比，Python的类机制为其添加了最少量的新语法和语义。Python类 提供了面向对象编程的所有标准功能: 类继承机制(inheritance mechanism)允许多个基类(base class)，派生类(derived class)可以重写其基类或类的任何方法，并且方法(method)可以调用具有相同名称的基类的方法。对象(object)可以包含任意数量和种类的数据。与模块一样，类也具有Python的动态特性: 它们是在运行时创建的，并且可以在创建后进一步修改。


<br>
<br/>


### 关于名称和对象

A Word About Names and Objects

Objects have individuality, 并且可以将多个名称(在多作用域中)绑定到同一个对象。这在其它语言中被称为别名。别名在不可变类型中被安全地忽略。但对涉及可变对象(dict, list...)的Python代码的语义可能会有惊人的影响。这通常有利于程序，因为别名在某些方面表现得像指针。


<br>
<br/>


### 作用域和命名空间

Python Scopes and Namespaces

类定义在命名空间中扮演一些巧妙的技巧，并且你需要知道作用域和命名空间如何工作才能完全理解正在发生的事情。顺便一提，有关此主题的知识对于任何高级Python程序员都很有用。

让我们从一些定义开始:

命名空间是名称到对象的映射。大多数命名空间目前都是作为Python字典实现的，但通常不会以任何方式显示。
命名空间的例子： 内建名称的集合；模块中的全局名称；函数调用中的本地名称。
从某种意义上说，对象的一组属性也构成一个命名空间。了解命名空间的重要之处在于，不同命名空间中的名称之间没有绝对的关系。

顺便一提，使用单词属性来表示任意一个点`.`后面的名称——`z.real`，real是对象z的属性。严格地说，对模块中的名称引用是属性引用——`modname.funcname`，modname是一个模块对象，并且funcname是它的一个属性。
在这种情况下，模块的属性和模块中定义的全局名称之间会有一个直接的映射关系: 它们共享相同的命名空间。

属性可以是只读或可写。

命名空间是在不同的时刻创建的，并且具有不同的生命周期。包含内建名称的命名空间是在Python解释器启动时创建的，并且永远不会被删除。读取模块定义时创建模块的全局命名空间，通常，模块命名空间也会持续到解释器退出。
由解释器的顶层调用执行的语句，无论是从脚本文件读取还是交互式读取，都被视为名为`__main__`模块的一部分，因此它们具有其自己的全局命名空间。

函数的本地命名空间是在调用函数时创建，并在函数返回时删除或引发(raise)不在函数内处理的异常。当然，递归调用每个都有自己的本地命名空间。

作用域(scope)是Python程序的文本区域，可以直接访问命名空间(namespace)。这意味着对名称的非限定引用(unqualified reference)会尝试在命名空间中查找名称。

尽管作用域是静态确定的，但它们是动态使用的。在执行期间的任何时候，至少有三个作用域的命名空间都可以直接访问:

- 最先搜索的最内层作用域，包含本地名称
- 从最近封闭作用域开始搜索的任何封闭函数的作用域，包含非本地名称，也包含非全局名称
- 倒数第二个作用域包含当前模块的全局名称
- 最外层的作用域是包含内建名称的命名空间

<br>

如果某个名称被声明为全局(global)，则所有的引用(reference)和赋值(assignment)都将直接转到包含模块全局名称的中间作用域。要重新绑定(rebind)最内层作用域外发现的变量，可以使用`nonlocal`语句；如果没有声明`nonlocal`，那些变量是只读的。

通常，本地作用域引用当前函数的本地名称。在外部函数中，本地作用域引用与全局作用域相同的命名空间:模块的命名空间。类定义在本地作用域中放置另一个命名空间。

认识到作用域是以文本方式确定是很重要的: 模块中定义的函数的全局作用域是该模块的命名空间，无论从何处调用函数或调用函数的别名。另一方面，名称的实际搜索是在运行时动态完成的——但是，在编译时间，语言定义正在向静态名称解析发展，因此不要依赖动态名称解析。

Python的特殊之处在于——如果`global`语句没有生效，对名称的赋值总是进入最内层的范围。赋值不会分配数据——它们只是将名称绑定到对象。删除操作也是如此: 语句`del x`从本地作用域引用的命名空间中删除x的绑定。实际上，所有引用新名称的操作都是用本地作用域: 特别是，`import`语句和函数定义将模块或函数名称绑定到本地作用域。

`global`声明可以用来表明特定变量存在于全局作用域内，应该在此rebound(反弹)。`nonlocal`声明表明特定变量存在于封闭作用域内，应该在那里rebound.


<br>


**作用域和命名空间的栗子**

```py
def scope_test():
    def do_local():
        spam = "local spam"

    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal spam"

    def do_global():
        global spam
        spam = "global spam"

    spam = "test spam"

    do_local()
    print("After local assignment: ", spam)

    do_nonlocal()
    print("After nonlocal assignment: ", spam)

    do_global()
    print("After global assignment: ", spam)

scope_test()
print("In global scope: ", spam)



#输出
After local assignment: test spam
After nonlocal assignment: nonlocal spam
After global assignment: nonlocal spam
In global scope: global spam
```


<br>
<br/>


### 首先看类

A first look at class

类引入了一些新的语法，三种新的对象类型和一些新的语义。


<br>


#### 类定义语法

Class Definition Syntax

类定义，像函数定义，必须在它们有效之前被执行。

```py
class ClassName:
    <statement-1>
    .
    .
    .
    <statement-N>
```

实际上，类定义中的语句通常是函数定义，但其他语句是允许的，有时也是有用的。类中的函数定义通常有一个特殊形式的参数列表，由方法的调用约定决定。

当输入一个类定义时，会创建一个新的命名空间，并将其用作本地作用域——因此，所有对局部变量的赋值都会进入这个新的命名空间。特别是，函数定义在此绑定新函数的名称。

当类定义保持正常时，会创建一个类对象。这基本上是由类定义创建的命名空间的内容的一个包装。最初的本地作用域被恢复，并且类对象在这里被绑定到类定义头中给出的类名。


<br/>


#### 类对象

Class Objects

类对象支持两种操作: 属性引用(attribute reference)和实例化(instantiation).

属性引用使用 用于Python中所有属性引用的标准语法: `obj.name`. 有效的属性名称在创建类对象时时位于类命名空间中的所有名称。

```py
class MyClass:
    """A simple example class"""
    i = 12345

    def f(self):
        return 'hello world'
```

`MyClass.i`和`MyClass.f`是有效的属性引用，分别返回一个整数和函数对象。类属性也可以被分配，所以也可以通过赋值来改变`MyClass.i`的值。`__doc__`也是一个有效的属性，返回该类的文档字符串"A simple example class".

类实例化使用函数表示法。假设类对象是一个返回类的新实例的无参数函数。

```py
#创建类的新实例，并将该对象分配给局部变量x
x = MyClass()
```

实例化操作("调用"一个类对象)创建一个空对象。许多类喜欢创建具有定制(customized)到特定初始状态(initial state)的实例对象。因此，类可以定义一个名为`__init__()`的特殊方法。

```py
def __init__(self):
    self.data = []
```

当一个类定义了一个`__init__()`方法时，类实例化会自动为新创建的类实例调用`__init__()`.

当然，`__init__()`方法可能有更多灵活的参数。在这种情况下，给类实例化操作符的参数被传递给`__init__()`.

```py
class Complex:
    def __init__(self, realpart, imagpart):
        self.r = realpart
        self.i = imagpart


x = Complex(3.0, -4.5)
x.r, x.i
3.0, -4.5
```


<br>


#### 实例对象

Instance Objects

实例对象理解的唯一操作是属性引用。有两种有效的属性名称，数据属性和方法。

数据属性不需要声明，像局部变量一样，当它们在第一次分配时就会弹出。
另一种实力属性引用是一种方法。方法是属于对象的函数。

实例对象的有效方法名称取决于它的类。根据定义，作为函数对象的类的所有属性都定义其实例的相应方法。


<br/>


#### 方法对象

Method Objects

关于方法的特殊之处在于 实例对象作为函数的第一个参数传递。一般来说，调用带有n个参数列表的方法相当于使用通过在第一个参数之前插入方法实例对象创建的参数列表来调用相应的函数。

当引用不是数据属性的实例属性时，将搜索类。如果名称表示一个有效的类属性，它是一个函数对象，则通过打包实例对象和在抽象对象中一起找到的函数对象来创建方法对象，这就是方法对象。当使用参数列表调用方法对象时，会从实例对象和参数列表构造一个新参数列表，并使用此新参数列表调用函数对象。


<br/>


#### 类变量和实例变量

Class and Instance Variables

一般来说，实例变量是针对每个实例唯一的数据，而类变量是针对类的所有实例共享的属性和方法。

```py
class Dog:

    kind = 'canine'         # class variable shared by all instances

    def __init__(self, name):
        self.name = name    # instance variable unique to each instance

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.kind                  # shared by all dogs
'canine'
>>> e.kind                  # shared by all dogs
'canine'
>>> d.name                  # unique to d
'Fido'
>>> e.name                  # unique to e
'Buddy'
```


共享数据可能会带来令人惊讶的影响，涉及列表和字典等可变对象:

```py
class Dog:

    tricks = []             # mistaken use of a class variable

    def __init__(self, name):
        self.name = name

    def add_trick(self, trick):
        self.tricks.append(trick)


>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.add_trick('roll over')
>>> e.add_trick('play dead')
>>> d.tricks                # unexpectedly shared by all dogs
['roll over', 'play dead']
```

正确的类设计应该使用实例变量:

```py
class Dog:

    def __init__(self, name):
        self.name = name
        self.tricks = []    # creates a new empty list for each dog

    def add_trick(self, trick):
        self.tricks.append(trick)

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.add_trick('roll over')
>>> e.add_trick('play dead')
>>> d.tricks
['roll over']
>>> e.tricks
['play dead']
```


<br/>
<br/>


### 随机备注

Random Remarks

数据属性覆盖具有相同名称的方法属性；为了避免意外的名称冲突，这可能会在大型程序中导致难以发现的错误，使用某种最小化冲突几率的约定是明智的。可能的约定(convention)包括: 大写的方法名称，小唯一字符串(可能只是下划线)为数据属性名称加前缀，或者为方法和名词使用动词来表示数据属性。

数据属性可由方法及对象的普通用户引用。换句话说，累不可用于实现纯粹的抽象数据类型。事实上，Python中没有任何东西可以强制执行数据隐藏——它都基于约定。

客户端应该小心使用数据属性。请注意，客户端可以将自己的数据属性添加到实例对象，而不会影响方法的有效性，只要避免名称冲突——再次注意，命名约定可在此节省大量令人头痛的问题。

从方法中引用数据类型没有简写，这增加了方法的可读性: 在浏览方法时，不会混淆局部变量和实例变量。

通常，方法的第一个参数称为`self`。这只不过是一个约定: 名字`self`对Python来说绝对没有特殊含义。但是，请注意，不遵循约定的Python代码对于Python程序员来说可能不易读取。

任何作为类属性的函数对象都为该类的实例定义了一个方法

```py
# Function defined outside the class
def f1(self, x, y):
    return min(x, x+y)

class C:
    f = f1

    def g(self):
        return 'hello world'

    h = g


#f, g, h都是类C的所有属性，它们都是指向函数对象的，因此它们都是C实例的所有方法。
```


方法可以通过使用`self`参数的方法属性来调用其它方法:

```py
class Bag:
    def __init__(self):
        self.data = []

    def add(self, x):
        self.data.append(x)

    def addtwice(self, x):
        self.add(x)
        self.add(x)
```

方法可以像普通函数一样引用全局名称。与方法关联的全局作用域是包含其定义的模块。(一个类永远不会被用作全局作用域) 虽然很少有人在方法中使用全局数据，但全局作用域有许多合法用途: 首先，导入全局作用域的函数和模块可以被方法使用，以及在其中定义的函数和类。通常，包含该方法的类本身是在全局作用域内定义的。

每个值都是一个对象，因此有一个类(类型)。它被存储为`object.__class__`


<br/>
<br/>


### 继承

Inheritance

当然，如果不支持继承，语言特性就不值得称为"类"。

```py
#派生(derived)
class DerivedClassName(BaseClassName):
    <statement-1>
    .
    .
    .
    <statement-N>
```

基类(BaseClassName)必须在包含派生类(derived class)定义的作用域中定义。代替基类名称，其它表达式也是允许的。

```py
#当基类在另一个模块中被定义
class DerivedClassName(modname.BaseClassName):
```

派生类(derived class)定义的执行过程与基类(base class)相同。当构造(constructed)类对象时，基类将被记住。这用于解析属性引用: 如果在类中未找到请求的属性，则搜索继续查找基类。如果基类本身是从其它类派生的，则此规则将递归应用。

派生类的实例化么有什么特别的: `DerivedXlassName()`创建一个新的类实例。方法解析如下: 如果需要，搜索相应的类属性，沿着基类链降序排列，如果产生函数对象，则方法引用是有效的。

派生类可以覆盖(override)基类的方法。由于方法在调用同一对象的其它方法时没有特殊的权限，因此调用另一个在同一基类中定义的方法的基类方法可能最终会调用派生类的方法来覆盖它。

派生类的覆盖(override)方法事实上可能需要扩展而不是简单地替换同名的基类方法。有一种简单的方法可以直接调用基类方法: 只需调用`BaseClassName.methodname(self, arguments)`即可。

<br>

Python有两个与继承有关的内建函数:

- `isinstance()`
检查一个实例的类型。`isinstance(obj, int)`只有在`obj.__class__`是int或从int派生的某个类时才为true

- `issubclass()`
检查类继承。


<br/>


#### 多重继承

Multiple Inheritance

Python支持多重继承的形式。

```py
class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    .
    .
    .
    <statement-N>
```

在最简单的情况下，你可以将从父类继承的属性视为深度优先(depth first)，从左到右搜索，而不是在同一个类中进行两次搜索，其中层次结构中存在重叠。
因此，如果在DerivedClassName中找不到属性，则在Base1中搜索该属性，然后(递归)在Base1的基类中搜索该属性。如果未找到，则在Base2中搜索该属性，依此类推。

动态排序是必要的，因为多重继承的情况都表现出一个或多个菱形关系。例如，所有类都从对象继承，所以任何多重继承的情况都会提供多条路径来达到对象。为了避免基类被多次访问，动态算法使搜索顺序线性化，以保留没各类众指定的从左到右的顺序，每个父类只调用一次，这是单调的。


<br/>



#### 私有变量

Private Variables

Python中不存在私有(private)实例变量，这些变量除了在对象内部以外不能访问。不过，大多数Python代码都有一个约定，以下划线`_spam`为前缀的名称应被视为API的非公共部分(无论是函数，方法或数据成员)。

由于私有类(class-private)成员有一个有效的用例(即为了避免名称与由子类定义的名称的冲突)，所以对这种称为`name mangling`的机制的支持有限。任何`__spam`形式的标识符在文本上用`_classname__spam`替换，其中`classname`是当前类名称，前导下划线被去除。只要它在类的定义类发生，就不会考虑标识符位置。

Name mangling 有助于让子类重写方法而不会破坏intraclass方法调用:

```py
class Mapping:
    def __init__(self, iterable):
        self.items_list = []
        self.__update(iterable)

    def update(self, iterable):
        for item in iterable:
            self.items_list.append(item)

    __update = update   # private copy of original update() method

class MappingSubclass(Mapping):

    def update(self, keys, values):
        # provides new signature for update()
        # but does not break __init__()
        for item in zip(keys, values):
            self.items_list.append(item)
```

请注意，强化规则的设计主要是为了避免事故；它仍然可以访问或修改被认为是私有的变量。
注意传递给`exec()`或`eval()`的代码并不认为调用类的类名是当前类；这与`global`语句的效果类似，其效果同样局限于一起进行字节编译的代码。`getattr()`, `setattr()`和`delattr()`以及直接使用`__dict__`时也有相同的限制。


<br/>
<br/>


### Odds and Ends

```py
class Employee:
    pass

john = Employee()  # Create an empty employee record

# Fill the fields of the record
john.name = 'John Doe'
john.dept = 'computer lab'
john.salary = 1000
```

一段期望特定抽象数据类型的Python代码通常通常可以传递一个模拟该数据类型方法的类。
例如，如果你有一个函数可以格式化文件对象中的某些数据，则可以使用方法`read()`和`readline()`来定义一个类，以便从字符串缓冲区总获取数据，然后将其作为参数传递。


<br/>
<br/>


### 迭代器

Iterators

你可能注意到大多数容器对象可以使用for语句循环遍历:

```py
for element in [1, 2, 3]:
    print(element)
for element in (1, 2, 3):
    print(element)
for key in {'one':1, 'two':2}:
    print(key)
for char in "123":
    print(char)
for line in open("myfile.txt"):
    print(line, end='')
```

这种访问方式清晰，简洁，方便。迭代器的使用贯穿并统一了Python。`for`语句在容器对象上调用`iter()`。该函数返回一个迭代器对象，该对象定义一次访问容器中元素的方法`__next__()`。当没有更多元素是，`__next__()`引发一个**StopIteration**异常，它告诉`for`循环终止。
你可使用`next()`内置函数调用`__next__()`方法:

```py
>>> s = 'abc'
>>> it = iter(s)
>>> it
<iterator object at 0x00A1DB50>
>>> next(it)
'a'
>>> next(it)
'b'
>>> next(it)
'c'
>>> next(it)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    next(it)
StopIteration
```

<br>

看到了迭代器协议背后的机制，很容易将迭代器行为添加到类中。定义一个`__iter__()`方法，该方法使用`__next__()`方法返回一个对象。

```py
class Reverse:
    """Iterator for looping over a sequence backwards."""
    def __init__(self, data):
        self.data = data
        self.index = len(data)

    def __iter__(self):
        return self

    def __next__(self):
        if self.index == 0:
            raise StopIteration
        self.index = self.index - 1
        return self.data[self.index]
```

```py
>>> rev = Reverse('spam')
>>> iter(rev)
<__main__.Reverse object at 0x00A1DB50>
>>> for char in rev:
...     print(char)
...
m
a
p
s
```


<br/>
<br/>


### 生成器

Generators

生成器是创建迭代器的简单而强大的工具。它们像常规函数一样编写，但只要它们想返回数据就是用`yield`语句。每次`next()`被调用时，生成器都会从停止的地方恢复(它记住所有的数据值以上次执行的代码)。

```py
def reverse(data):
    for index in range(len(data)-1, -1, -1):
        yield data[index]


>>> for char in reverse('golf'):
...     print(char)
...
f
l
o
g
```

任何可用生成器完成的事情也可用前面的基于类的迭代器完成。使生成器如此紧凑的原因是`__iter__()`和`__next__()`方法时自动创建的。

另一个关键特性是本地变量和执行状态在调用之间自动保存。这使得该函数更容易编写，并且比使用`self.index`和`self.data`等实例变量的方法更加清晰。
除了自动方法创建和保存程序状态之外，当生成器终止时，它们会自动产生StopIteration。结合起来，这些功能可以轻松创建迭代器，而无需编写常规函数。


<br/>
<br/>


### 生成器表达式

Generator Expressions

一些简单的生成器可以使用与列表解析类似的语法简洁地编码为表达式，带括号而不是方括号。这些表达式适用于通过封闭函数立即使用生成器的情况。
生成器表达式比完整的生成器定义更紧凑但功能更少，并且倾向于比等效的列表解析更具有内存友好性。

```py
>>> sum(i*i for i in range(10))                 # sum of squares
285

>>> xvec = [10, 20, 30]
>>> yvec = [7, 5, 3]
>>> sum(x*y for x,y in zip(xvec, yvec))         # dot product
260

>>> from math import pi, sin
>>> sine_table = {x: sin(x*pi/180) for x in range(0, 91)}

>>> unique_words = set(word  for line in page  for word in line.split())

>>> valedictorian = max((student.gpa, student.name) for student in graduates)

>>> data = 'golf'
>>> list(data[i] for i in range(len(data)-1, -1, -1))
['f', 'l', 'o', 'g']
```



<br>
<br/>

---

<br/>



## 虚拟环境

Virtual Environments and Packages

应用程序有时候需要特定的模块版本，或者某个模块只支持特定Python版本。
这就意味着一个Python安装版本可能无法满足每个应用程序的要求。(如某个应用程序支持Python2.7，而某个应用程序支持Python3.x)

此问题的解决方案是创建一个虚拟环境(virtual environment)——一个包含特定Python安装包和软件包的目录树。
这样，不同的应用程序就可以使用不同的虚拟环境。


<br/>
<br/>


### 创建虚拟环境

Creating Virtual Environments

用于创建和管理虚拟环境额模块称为`venv`.它通常会为你安装最新版本的Python，你也可以选择Python版本。

激活虚拟环境后，会改变提示符并修改环境，以便提供特定的Python版本。

```
#创建虚拟环境
python3 -m venv /tmp/pythonVenv

#激活
source /tmp/pythonVenv/bin/activate
(pythonVenv) [zhang@zhang21 ~]$

(pythonVenv) [zhang@zhang21 ~]$ python
>>> import sys
>>> sys.path
['', '/usr/lib64/python34.zip', '/usr/lib64/python3.4', '/usr/lib64/python3.4/plat-linux', '/usr/lib64/python3.4/lib-dynload', '/tmp/pythonVenv/lib64/python3.4/site-packages', '/tmp/pythonVenv/lib/python3.4/site-packages']


#退出
deactivate
```


<br/>
<br/>


### pip包管理

你可以使用pip程序进行搜索、安装、升级和移除软件包。pip程序默认从<pypi.org>安装软件包。

`pip freeze` 以requirements的格式输出已安装软件包。这很重要。

```
pip search sh

#默认安装最新版本
pip install sh

#安装指定版本
pip install sh=1.10.2

pip install --upgrade sh

pip uninstall sh

#显示已安装的模块的详细信息
pip show sh

#列出已安装模块
pip list


pip freeze > requirements.txt
#安装依赖
pip install -r ./requirements.txt
```


<br/>


#### 配置国内源

Linux:

```sh
# global

vim ~/.pip/pip.conf

# aliyun
[global]
trusted-host=mirrors.aliyun.com
index-url=https://mirrors.aliyun.com/pypi/simple/



# temporary
pip install xxx -i https://mirrors.aliyun.com/pypi/simple/
```



<br/>
<br/>
<br/>




## 下划线


参考:

- <https://shahriar.svbtle.com/underscores-in-python>
- <https://segmentfault.com/a/1190000002611411>
- <https://zhuanlan.zhihu.com/p/36173202>

<br>

本节讨论Python中下划线(`_`)的使用，它的大部分用法都是一种惯例约定。

| 模式 | 栗子 | 含义 |
| - | - | - |
| 单下划线前缀 | `_var` | 命名约定，仅供内部使用。通常不会有Python解释器强制执行，只作为对程序员的提示 |
| 单下划线后缀 | `var_` | 按约定使用以避免与Python关键字的命名冲突 |
| 双下划线前缀 | `__var` | 当在类上下文中使用时，触发名称修饰 |
| 双下划线前后缀 | `__var__` | 表示Python语言定义的特殊方法 |
| 单个下划线 | `_` | 三个情况 |

<br/>


### 单个下划线

单下划线(`_`)主要有三种情况:

- **解释器中**
下划线(`_`)符号指交互式解释器中最后一次执行语句的返回结果。

```
>>> _
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name '_' is not defined
>>>
>>> 111
111
>>>
>>> _
111
```

- **作为名称使用**
下划线(`_`)用作被丢弃的名称。这样可以让阅读你代码的人知道，这是个不会被使用的特定名称。

- **国际化**
下划线(`_`)用作函数名。这种情况下，单下划线经常被用作国际化和本地化字符串翻译查询的函数名。
在Django中，你可能会看到:

```py
from django.utils.translation import ugettext as _
from django.http import HttpResponse

def my_view(request):
    output = _("Welcome to my site.")
    return HttpResponse(output)
```


<br/>
<br/>


### 单下划线前缀的名称

以单下划线做前缀的名称(如`_shahriar`)，指定了这个名称是**私有的**。
在有些`import *`的场景中，下一个使用你代码的人会明白这个名称仅供内部使用。
下划线前缀的含义是告知其他程序员：以单个下划线开头的变量或方法仅供内部使用。 该约定在PEP 8中有定义。

如果你写了`from module import *`，那么以单下划线开头的名称都不会被导入，除非模块或包中的`__all__`列表显式地包含了它们。


<br/>
<br/>


### 单下划线后缀的名称

以单下划线后缀的名称(如`var_`)，有时，一个变量的最合适的名称已被一个关键字所占用。在这种情况下，你可以附加一个下划线来解决命名冲突。

```
>>> def make_object(name, class):
SyntaxError: "invalid syntax"

>>> def make_object(name, class_):
...    pass
```


<br/>
<br/>


### 双下划线前缀的名称

以双下划线做前缀的名称(如`__shahriar`)，它对解释器有特定含义。Python中的这种用法是为了避免与子类定义的名称冲突。


<br/>
<br/>


### 前后都有双下划线的名称

前后都有双下划线的名称(如`__init__`)，是Python的特殊方法名，这是一种惯例，一种确保Python系统中的名称不会跟用户自定义的名称发生冲突的方式。




<br>
<br/>

---

<br>



# 语言参考

The Python Language Reference

Python参考手册描述了Python语言的**语法(syntax)**和**核心语义(core semantics)**。



<br/>
<br/>



## 介绍

Introduction


此参考手册描述了Python编程语言，它并不是一个教程。
如果你正在使用Python并且想知道关于改语言的特定区域的精确规则是什么，那么你绝对应该能够在这里找到它们。


<br/>
<br/>


## 词法分析

Lexical analysis


解析器(parser)读取Python程序。解析器的输入是由词法分析器生成的令牌流(stream of tokens)。本章描述了词法解析器如何将文件分解为令牌。

Python将程序文本读作`Unicode code point`，源文件的编码可以通过编码声明给出，默认为`UTF-8`，具体请参阅[PEP 320](https://www.python.org/dev/peps/pep-3120/)。如果源文件无法被编码，则抛出语法错误。


<br/>
<br/>


### 行结构

Line structure

Python程序分为许多逻辑行。

<br>

- **逻辑行**
Logical lines

逻辑行的结尾由token NEWLINE表示。语句不能跨过逻辑行边界，除非语法允许NEWLINE。通过遵循显式或隐式的行连接规则，从一个或多个物理行构造逻辑行。

<br>

- **物理行**
Physical lines

物理行是由行尾序列终止的字符序列。在源文件和字符串中，可使用任何标准平台的行终止序列。Unix格式使用ASCII的`LF`，Windows格式使用ASCII的`CR LF`，或使用旧的Macintosh格式ASCII `CR`字符。无论平台如何，所有这些格式都可以平等使用。输入的结尾也充当最终物理行的隐式终止符。
嵌入Python时，应使用标准C约定的换行符将源代码字符传递给Python API。

<br>

- **注释**
Comments

注释以哈希字符(`#`)开头，以物理行的末尾结束。注释表示逻辑行的结束，除非调用隐式行连接规则。语法会直接忽略注释。

<br>

- **编码声明**
Encoding declarations

如果Python脚本中的第一行或第二行中的注释与正则表达式`coding[=:]\s*([-\w.]+)`相匹配，则此注释将作为编码声明处理。编码声明必须出现在它自己的一行上。若果是第二行，则第一行也必须是仅注释行。
编码表达式的推荐格式:

```py
# -*- coding: <encoding-name> -*-
```

如果未发现编码声明，默认编码为`UTF-8`。
如果声明了编码，则必须有Python识别编码名称。编码用于所有词法分析，包括字符串文字，注释和标识符。

<br>

- **显式行连接**
Explicit line joining

可使用反斜杠(`\`)将两个或多个物理行连接到逻辑行中。

```py
if 1900 < year < 2100 and 1 <= month <= 12 \
  and 1 <= day <= 31 and 0 <= hour < 24 \
  and 0 <= minute < 60 and 0 <= secod < 60:    #Look like a valid date
    return 1
```

以反斜杠结尾的行不能编写注释。反斜杠在字符串文字外的一行上的其他位置是非法的。

<br>

- **隐式行连接**
Implicit line joining

括号，方括号，花括号中的表达式可以在不使用反斜杠的情况下分割为多个物理行。

```py
month_names = ['January', ‘February', 'March',    #comments
               'April', 'May', 'June',            #comments
			   'July', 'Auguest', 'September',    #comments
			   'October, 'November', 'December']
```

隐式的连续行可以带有注释，连续行的缩进并不重要。允许空白的连续行。

<br>

- **空白行**
Blank lines

包含空格，制表符，换页符，注释的逻辑行会被忽略。在标准的交互式解释器中，完全空白的逻辑行终止多行语句。

<br>

- **缩进**
Indentation

逻辑行开头的前导空白(空格和制表符)用于计算行的缩进级别，而后者又用于语句的分组。

`tabs`被1-8个空格替换(从左到右)，使得包括被替换的字节数总是八的倍数。第一个非空白字符前面的空格总数确定行的缩进。缩进不能够使用反斜杠在多个物理行上分隔。
如果源文件以一种方式混合制表符(`tab`)和空格，使得含义取决于空格中制表符的价值，则缩进被拒绝为不一致。会抛出`TabError`异常。

跨平台兼容性说明： 由于non_Unix平台上文本编辑器的性质，在源文件中使用制表符和空格的混合来缩进是不明智的。还应注意，不同平台可以明确地限制最大缩进级别。


<br/>
<br/>


### 标识符和关键字

Identifiers and keywords


标识符也称为名称，标识符的长度不受限制。
Python中标识符的语法基于Unicode标准附件UAX-31，详情请参考[PEP 3131](https://www.python.org/dev/peps/pep-3131/)
在ASCII范围内(`U+0001...U+007F`)，有效的字符与Python2相同。大小写字母`A-z`，除第一个字符外的下划线(`_`)，数字`0-9`。
Python3引入了ASCII范围外的其它字符，对于这些字符，分类使用`unicodedata`模块中包含的Unicode Character Database的版本。

Unicode类别代码表示：

- Lu： uppercase letters
- Ll： lowercase letters
- Lt： titlecase letters
- Lm： modifier letters
- Lo： other letters
- Nl： letter numbers
- Mn： nonspacing marks
- Mc： spacing combining marks
- Nd： decimal numbers
- Pc： connector punctuations
- Other_ID_Start： explicit list of characters in PropList.txt to support backwards compatibility
- Other_ID_Continue： likewise

<br>

**关键字**
Keywords

以下标识符用作保留字或关键字，不能用作普通标识符。

```
help(keywords)

False  class     finally  is        return
None   continue  for      lambda    try
True   def       from     nonlocal  while
and    del       global   not       with
as     elif      if       or        yield
assert else      import   pass
break  except    in       raise
```

<br>

**保留的类标识符**
Reserved classes of identifiers

某些类标识符(除了关键字)具有特殊含义。这些类由前导/后置下划线(`_`)字符标识：

- `_*`
特殊标识符`_`，用于交互式解释器中存储上次评估的结果，它保存在内建模块中。当不处于交互式模式时，下划线`_`没有特殊含义，也没有定义。
名称`_`通常与国际化一起使用，这是一种约定。

- `__*__`
系统定义的名称。这些名称由解释器及其实现(包括标准库)来定义。在任何情况下，任何使用`__*__`名称都不会明确记录，在没有任何警告的情况下会受到破坏。

- `__*`
私有类(class-private)名称。在类定义的上下文中使用中使用此目录中的名称，名称将被重写，以使用损坏的格式来帮助避免基类和派生类(base and derived class)的私有属性之间的名称冲突。


<br/>
<br/>


### 文字值

Literals


文字值是一些内建类型的常量值的符号。

<br>

**字符串和字节文字值**
String and Bytes literals

字符串文字值和字节文字值描述：

```
stringliteral   ::=  [stringprefix](shortstring | longstring)
stringprefix    ::=  "r" | "u" | "R" | "U" | "f" | "F"
                     | "fr" | "Fr" | "fR" | "FR" | "rf" | "rF" | "Rf" | "RF"
shortstring     ::=  "'" shortstringitem* "'" | '"' shortstringitem* '"'
longstring      ::=  "'''" longstringitem* "'''" | '"""' longstringitem* '"""'
shortstringitem ::=  shortstringchar | stringescapeseq
longstringitem  ::=  longstringchar | stringescapeseq
shortstringchar ::=  <any source character except "\" or newline or the quote>
longstringchar  ::=  <any source character except "\">
stringescapeseq ::=  "\" <any source character>



bytesliteral   ::=  bytesprefix(shortbytes | longbytes)
bytesprefix    ::=  "b" | "B" | "br" | "Br" | "bR" | "BR" | "rb" | "rB" | "Rb" | "RB"
shortbytes     ::=  "'" shortbytesitem* "'" | '"' shortbytesitem* '"'
longbytes      ::=  "'''" longbytesitem* "'''" | '"""' longbytesitem* '"""'
shortbytesitem ::=  shortbyteschar | bytesescapeseq
longbytesitem  ::=  longbyteschar | bytesescapeseq
shortbyteschar ::=  <any ASCII character except "\" or newline or the quote>
longbyteschar  ::=  <any ASCII character except "\">
bytesescapeseq ::=  "\" <any ASCII character>
```

两种类型的文字值都可用单引号(`'`)或双引号(`"`)括起来，也能包含在三个引号中。反斜杠(`\`)字符用于转义好友特殊含义的字符。
字节文字值总是以`b`或`B`为前缀，它们生成byte类型的实例，而不是str类型。它们可能只包含ASCII字符(128)，更大的字节必须转义。
字符串和字节文字值都可以选择以字母`r`或`R`为前缀，如原始字符串将反斜杠视为文字字符。因此，在字符串文字值中，原始字符串中的`\u`和`\U`不会被特殊处理。

公认的转义序列：

| Escape Sequence | Meaning |
| - | - |
| `\newline` | Backslash and newline ignored |
| `\\` | Backslash (\) |
| `\'` | Single quote (') |
| `\"` | Double quote (") |
| `\a` | ASCII Bell (BEL) |
| `\b` | ASCII Backspace (BS) |
| `\f` | ASCII Formfeed (FF) |
| `\n` | ASCII Linefeed (LF) |
| `\r` | ASCII Carriage Return (CR) |
| `\t` | ASCII Horizontal Tab (TAB) |
| `\v` | ASCII Vertical Tab (VT) |
| `\ooo` | Character with octal value ooo |
| `\xhh` |  Character with hex value hh |

仅在字符串文字值中识别的转义序列：

| Escape Sequence | Meaning |
| - | - |
| `\N{name}` | Character named name in the Unicode database |
| `\uxxxx` | Character with 16-bit hex value xxxx |
| `\Uxxxxxxxx` | Character with 32-bit hex value xxxxxxxx |

<br>

**字符串文字串联**
String literal concatenation

多个相邻的字符串或字节文字值(由空格分隔)，可能使用不同的引用约定，并且它们的含义与它们的串联相同。因此，`"hello" 'world'`等同于`"helloworld"`。此功能可用于减少反斜杠的数量，方便地跨长行分隔长字符串，甚至可以为字符串的某些部分添加注释。

```
re.compile("[A-Za-z]"      #letter or underscore
           "[A-Za-z0-9_]*"  #letter, digit or underscore
          )
```

注意，此功能在语法级别上定义，但在编译时实现。必须使用`+`操作符在运行时连接字符串表达式。

<br>

**格式化的字符串文字值**
Formatted string literals

格式化的字符串文字值是以`f`或`F`为前缀的字符串文字，这些字符串可能包含替换字段——由`{}`分隔的表达式。

```
f_string          ::=  (literal_char | "{{" | "}}" | replacement_field)*
replacement_field ::=  "{" f_expression ["!" conversion] [":" format_spec] "}"
f_expression      ::=  (conditional_expression | "*" or_expr)
                         ("," conditional_expression | "," "*" or_expr)* [","]
                       | yield_expression
conversion        ::=  "s" | "r" | "a"
format_spec       ::=  (literal_char | NULL | replacement_field)*
literal_char      ::=  <any code point except "{", "}" or NULL>
```

栗子：

```
>>> name = "Fred"
>>> f"He said his name is {name!r}."
"He said his name is 'Fred'."
>>> f"He said his name is {repr(name)}."  # repr() is equivalent to !r
"He said his name is 'Fred'."
>>> width = 10
>>> precision = 4
>>> value = decimal.Decimal("12.34567")
>>> f"result: {value:{width}.{precision}}"  # nested fields
'result:      12.35'
>>> today = datetime(year=2017, month=1, day=27)
>>> f"{today:%B %d, %Y}"  # using date format specifier
'January 27, 2017'
>>> number = 1024
>>> f"{number:#0x}"  # using integer format specifier
'0x400'
```

<br>

**数字文字值**
Numeric literals

有三种类型的数字文字值：

- integers
- floating point numbers
- imaginary numbers

没有复数文字值。
请注意，数字文字值不包含符号。像`-1`实际是由一元运算符`-`和文字值`1`组成的表达式。

**整数文字值**
除了可以存储在可用内存中之外，整数文字值的长度没有限制。

```
integer      ::=  decinteger | bininteger | octinteger | hexinteger
decinteger   ::=  nonzerodigit (["_"] digit)* | "0"+ (["_"] "0")*
bininteger   ::=  "0" ("b" | "B") (["_"] bindigit)+
octinteger   ::=  "0" ("o" | "O") (["_"] octdigit)+
hexinteger   ::=  "0" ("x" | "X") (["_"] hexdigit)+
nonzerodigit ::=  "1"..."9"
digit        ::=  "0"..."9"
bindigit     ::=  "0" | "1"
octdigit     ::=  "0"..."7"
hexdigit     ::=  digit | "a"..."f" | "A"..."F"
```

**浮点数文字值**

```
floatnumber   ::=  pointfloat | exponentfloat
pointfloat    ::=  [digitpart] fraction | digitpart "."
exponentfloat ::=  (digitpart | pointfloat) exponent
digitpart     ::=  digit (["_"] digit)*
fraction      ::=  "." digitpart
exponent      ::=  ("e" | "E") ["+" | "-"] digitpart
```

**虚数文字值**

```
imagnumber ::=  (floatnumber | digitpart) ("j" | "J")
```


<br/>
<br/>


### 运算符

Operators


```
+       -       *       **      /       //      %      @
<<      >>      &       |       ^       ~
<       >       <=      >=      ==      !=
```


<br/>
<br/>


### 分隔符

Delimiters


```
(       )       [       ]       {       }
,       :       .       ;       @       =       ->
+=      -=      *=      /=      //=     %=      @=
&=      |=      ^=      >>=     <<=     **=
```

Python中以下ASCII字符有重要意义：

```
'       "       #       \
```

Python中不使用以下ASCII字符:

```
$    ?
```



<br/>
<br/>
<br/>



## 数据模型

[Data model](https://docs.python.org/3.6/reference/datamodel.html)


<br/>


### 对象，值和类型

Objects, values and types


**对象(Objects)**是Python的数据抽象。Python程序中的所有数据都由对象或对象之间的关系表示。
每个对象都有一个标识(Identity)，一个类型(Type)和一个值(Value)。对象的标识一旦创建就永远不会改变。`is`操作符就是比较两个对象的标识；`id()`函数返回一个表示其标识的整数。

```python
name = 'zhang'
NAME = 'ZHANG'

name is NAME
False
```

CPython中，`id(x)`是存储x的内存地址。

对象的**类型(Type)**确定了对象支持的操作，并且还定义了该类型的对象的可能值。`type()`函数返回对象的类型。与对象标识一样，对象的类型也是不可更改的。

某些对象的**值(Value)**可以改变。值可以改变的对象被认为是可变的(mutable)，值不可改变的的对象被称为是不可变的(immutable)。对象的可变性由其类型决定。例如，数字(number)，字符串(string)，元组(tulple)是不可变的，字典(dictionary)和列表(list)是不可变的。

对象永远不会被明确销毁，然而，当它们变得无法到达(unreachable)时，它们可能会被垃圾回收(garbage-collected)。允许实现推迟垃圾回收或完全省略垃圾回收——只要没有回收到仍然是可以访问的对象，垃圾回收的实现方式就是如此。

请注意，使用实现的`tracing`或`debugging`工具可以使对象保持活动状态，这些对象通常是可回收的。另请注意，使用`try...except`语句捕获异常可能会使对象保持活动状态。

某些对象包含对外部(external)资源的引用，如打开的文件。当对象被垃圾回收时，这些资源被释放，但由于不能保证垃圾回收发生，这些对象还提供了一种释放外部资源的明确的方法——`close()`方法。强烈建议程序明确关闭此类对象。`try...finally`语句和`with`语句提供了方便的方法。

一些对象包含对其它对象的引用，这被称为**容器(container)**，容器的栗子是元组，列表和字典。引用是容器值的一部分。在大多数情况下，当我们谈论容器的值时，指的是值而并非容器对象的标识；但是，但我们谈论容器的可变性时，只隐含了直接包含的对象的标识。因此，如果一个不可变容器(如元组)包含对可变对象的引用，则如果更改了可变对象，则其值会改变。

类型几乎影响对象行为的所有方面。在某种意义上，即使对象标识的重要性也会受到影响：对于不可变类型，计算新值的操作实际上可以返回对具有相同类型和值的任何对象的引用，而对于可变对象，这是不允许的。



<br/>
<br/>
<br/>



### 标准类型层次结构

The standard type hierarchy


下面列出了Python內建的类型，扩展模块可定义其它类型。
下面的一些类型描述包含一个列出的`special attributes`段落。这些属性提供对实现的访问，不适用于一般用途。它们的定义未来可能发生变化。

<br>

**None**
此类型具有单个值(single value)。有一个具有此值的对象，可通过內建名称`None`访问此对象。在许多情况下它用于表示缺少值，例如，它是未明确返回任何内容的函数的返回。Its truth value is false.

<br>

**NotImplemented**
此类型具有单个值。有一个具有此值的对象，可通过內建名称`NotImplemented`访问此对象。如果数值方法和富比较方法尉氏县所提供的操作数的操作，则返回此值。Its truth value is true.

<br>

**Ellipsis**
此类型具有单个值。有一个具有此值的对象，可通过`...`或內建名称`Ellipsis`访问此对象。Its truth value is true.

<br>

**numbers.Number**
由数字创建，并由算术运算符和算术内置函数作为结果返回。数值对象是不可变的。Python数字与数学数字密切相关，但收到计算机中数值表示的限制。
Python区分整数(integer)、浮点数(floating point number)和复数(complex number)：

- **numbers.Integral**
  表示整数的数学集合(正数和复数)的元素，有两种类型的整数:
  - **Integers (int)**
  这代表无限范围内(unlimited range)的数字，仅受到可用(virtual)memory的限制。
  - **Booleans (bool)**
  这代表真值的`True`和`False`，这两个对象是唯一的布尔值对象。布尔类型是整数类型的子类型，布尔值在几乎所有上下文中的行为分别类似于值0和1，例外的是，当转换为字符串时，分别返回字符串`False`或`True`。

- **numbers.Real (float)**
这代表机器级双精度(double precision)浮点数。您可以接受底层机器架构，以获得可接受的范围和溢出处理。Python不支持单精度浮点数，没有理由使用两种浮点数复杂化语言。

- **numbers.Complex (complex)**
这将复数表示为一对机器级双精度浮点数。可通过只读属性`z.real`和`z.imag`来检索复数z的实部(real)和虚部(imaginary)。

<br>

**Sequences**
这表示由非负数索引的有限有序集。內建函数`len()`返回序列的项数。当序列长度为n时，索引集数字为`0, 1, ..., n-1`。序列的i项由`a[i]`选择。
序列还支持切片(slicing): `a[i:j]`选择一定范围内的项。当使用表达式时，切片是相同类型的序列。
一些序列还支持带有步进(step)参数的扩展切片: `a[i:j:k]`。
序列根据其可变性进行区分：

- **Immutable sequences**
不可变序列类型的对象一旦创建就不能更改。如果对象包含对其它对象的引用，则这些其它对象可能是可变的并且可能会被更改；但是，由不可变对象直接引用的对象集合不能更改。
不可变序列有以下类型：
  - **Strings**
  字符串是表示Unicode code ponit的值序列。`U+0000 - U+10FFFF`范围内的代码点都可以用字符串表示。Python没有`char`类型；相反，字符串中的每个代码点都表示为长度为1的字符串对象。內建函数`ord()`将代码点从字符串形式转换为`0-10FFFF`范围内的整数；`chr()`将`0-10FFFF`范围内的整数转化为对应长度为1的字符串对象；`str.encode()`可用于将`str`转换为`bytes`，而`bytes.decode()`用于实现相反的操作。
  ```python
  >>> ord('a')
  97
  >>> chr(97)
  a
  >>> str.encode('a')
  b'a'
  >>> bytes.decode(b'a')
  a
  ```
  - **Tuples**
  元组的项是任意Python对象。两个或多个项的元组由逗号分隔的表达式列表组成。空元组可由一对空括号组成。
  - **Bytes**
  字节对象是一个不可变的数组。这些项是8-bit bytes，由`0-255`范围内的整数表示。

- **Mutable sequences**
可变序列创建之后可以更改。订阅和切片表示法可用作赋值和`del`语句的目标。
目前有两种可变序列类型：
  - ***Lists*
  列表的项是任意Python对象。通过将逗号分隔的表达式放在方括号中来形成列表。
  - **Byte Arrays**
  bytearray对象是一个可变数组。它们由內建的`bytearray()`构造器(constructor)创建。

扩展模块`array`提供了可变序列类型的另一个示例，`collections`模块也是如此。

<br>

**Set types**
这代表无序，有限的唯一不可变的对象集。因此，它们不能被任何下标索引。但是，它们可以迭代，內建函数`len()`返回集合的项目数。集合的常见用途是进行快速成员资格测试，从序列中删除重复项，以及计算数学运算(交集、并集、差集...)。
对于集合元素，相同的不可变性规则适用于字典的键。请注意，数字类型遵循数字比较的常规规则：如果两个数字相等(如 1, 1.0)，则它们中只能包含其中一个。
目前有两种固有的集合类型:

- **Sets**
这代表一个可变集合。它们通过內建的`set()`构造器进行创建，之后可通过多种方法进行修改(如 `add()`)。

- **Frozen sets**
这代表一个不可变集合。它们通过內建的`frozenset()`构造器进行创建。它是可变且可清除的，因此它可以再次用作另一个集合的元素，或作为字典的键。

<br>

**Mappings**
这表示由任意索引集合索引的有限对象集。如下标符号`a[k]`从映射a中选择由k索引的项；这可在表达式中使用，也可作为赋值或`del`语句的目标。內建函数`len()`返回映射中的项数。
目前有一种内在映射类型：

- **Dictionaries**
这表示有几乎任意值索引的有限对象集。唯一不能作为键接受的值是包含列表或字典或其它可变类型的值，这些值通过值而不是按对象标识进行比较，原因是字典的有效实现需要键的哈希值保持不变。用于键的数字类型遵循用于数字比较的正常规则：如果两个数字相等(如1, 1.0)，则它们互相地用于索引相同的字典项。
字典是可变的，它们可通过`{...}`符号创建。
扩展模块`dbm.ndbm`和`dbm.gnu`提供了映射类型的其它实例，`collections`模块也是如此。

<br>

**Callable types**
这是可应用函数调用操作的类型：

- **User-defined functions**
用户定义的函数对象由函数定义创建。它应使用包含于函数的形式参数列表相同数量的项的参数列表来调用它。
特殊属性(special attributes):

| Attribute | Meaning | - |
| - | - | - |
| `__doc__` | The function’s documentation string, or `None` if unavailable; not inherited by subclasses | Writable |
| `__name__` | The function’s name | Writable |
| `__qualname__` | The function’s qualified name. New in version 3.3. | Writable |
| `__module__` | The name of the module the function was defined in, or `None` if unavailable. | Writable |
| `__defaults__` | A tuple containing default argument values for those arguments that have defaults, or `None` if no arguments have a default value | Writable |
| `__code__` | The code object representing the compiled function body. | Writable |
| `__globals__` | A reference to the dictionary that holds the function’s global variables — the global namespace of the module in which the function was defined. | Read-only |
| `__dict__ `| The namespace supporting arbitrary function attributes. | Writable|
| `__closure__` | `None` or a tuple of cells that contain bindings for the function’s free variables. | Read-only |
| `__annotations__` | A dict containing annotations of parameters. The keys of the dict are the parameter names, and `return` for the return annotation, if provided. | Writable |
| `__kwdefaults__` | A dict containing defaults for keyword-only parameters. | Writable |

标记为可写的大多数属性都会检查指定值的类型。
函数对象同样支持获取和设置任意属性。如，可使用这些属性将元数据附加到函数。常规属性`.`点表示法用于获取和设置此类属性。请注意，当前实现仅支持用户定义函数的函数属性。将来可能会支持内置函数的函数属性。

<br>

- **Instance methods**
实例方法对象组合了类(class)，类实例(class instance)和任何可调用对象(通常是用户定义的函数)。

特殊的只读属性：
`__self__`是类实例对象，`__func__`是函数对象；
`__doc__`是方法的文档(同`__func__.__doc__`)；
`__name__`是方法名(同`__func__.__name__`)；
`__module__`是定义的方法的模块的名称，如果不可用，则为`None`。

方法也支持访问底层函数对象上的任意函数属性。

当获取类的属性时，如果该属性是用户定义的函数对象或类方法对象，则可创建用户定义的方法对象。
当通过实例从类中检索用户定义的函数对象来创建实例方法对象时，其`__self__`属性是实例，并且方法对象被称为绑定(bound)。新方法`__func__`属性是原始函数对象。
当通过从类或实例检索另一个方法对象来创建用户定义的方法对象时，行为与函数对象的行为相同，只是新实例的`__func__`属性不是原始方法对象，而是其`__func__`属性。
当通过从类或实例检索类方法对象来创建实例方法对象时，其`__self__`属性是类本身，其`__func__`属性是类方法的基础函数对象。
调用实例方法对象时，将调用基础函数`__func__`，将类实例`__self__`插入参数列表前面。

注意，每次从实例检索属性时，都会发生从函数到实例方法对象的转换。在某些情况下，有成效的优化将属性分配给局部变量并调用局部变量。还要注意，此转换仅适用于用户定义的函数；在不进行转换的情况下检索其它可调用对象(以及不可调用对象)。作为类实例属性的用户定义函数不会转换为绑定方法;这只有在函数是类的属性时才会发生。

<br>

- **Generator functions**
使用`yield`语句的函数或方法被称为生成器函数。这样的函数在被调用时总是返回一个迭代器对象，它可以用来执行函数体：调用迭代器的`iterator.__next__()`方法将导致函数执行，直到它使用`yield`语句提供一个值。当函数执行`return`语句或结束时，会引发`StopIteration`异常，并且迭代器将到达要返回的值集合的末尾。

<br>

- **Coroutine functions**
使用`async def`定义的函数或方法称为协程函数。这样的函数在被调用时返回一个协程对象。它可能包含`await`表达式，以及`async with`和`async for`语句。

<br>

- **Asynchronous generator functions**
使用`async def`定义并使用`yield`语句的函数或方法称为异步生成器函数。这样的函数在调用时返回一个异步迭代器对象，该对象可在`async for`语句中用于执行函数体。

调用异步迭代器的`aiterator.__anext__()`方法将返回一个awaitable来等待执行，直到它使用`yield`表达式。当函数执行一个空的`return`语句或从结尾处掉落时，会引发`StopAsyncIteration`异常，异步迭代器将到达要生成的值集的末尾。

<br>

- **Built-in functions**
內建函数对象是C函数的包装器。

<br>

- **Built-in methods**
这实际上是内建函数的不同伪装，包含作为隐式额外参数传递给C函数的对象。

<br>

- **Classes**
类是可调用的。这些对象通常作为新实例的工程，但是对于覆盖`__new__()`的类类型可能存在变化。调用的参数传递给`__new__()`，在典型情况下，传递给`__init__()`以初始化新实例。

<br>

- **Class Instances**
通过在类中定义`__call__()`方法，可使任意类的实例可调用。

<br>

**Modules**
模块是Python代码的基本组织单元，由导入系统(import system)创建，由`import`语句调用。模块对象具有由字典对象实现的命名空间(由模块中定义的函数的`__globals__`属性引用)。属性引用被转换为该字典中的查找，如`m.x`相当于`m.__dict__['x']`。模块对象不包含用于初始化模块的代码对象。
属性赋值更新模块命名空间字典，如`m.x = 1`等效于`m.__dict__['x'] = 1`。

预定义(可写)属性：
- `__name__`是模块的名称；
- `__doc__`是模块的文档字符串，不可用为`None`。特殊的只读属性；
- `__annotations__`是包含在模块正文执行期间收集的变量注释的字典；
- `__file__`是从加载模块的文件的路径名，如果是从文件加载的。

<br>

**Custom classes**
自定义类类型通常由类定义创建。类具有由字典对象实现的命名空间。类属性引用被转换为此字典中的查找，如`C.x`被翻译为`C.__dict__['x']`。
类属性赋值更新类的字典，而不是基类(base class)的字典。
可调用类对象以生成(yield)类实例。

特殊属性：
- `__name__`是类名；
- `__module__`是类定义的模块名；
- ` __dict__`是包含类的命名空间的字典；
- `__bases__`是包含基类的元组，按它们在基类列表中出现的顺序排列；
- ` __doc__`是类的文档字符串，不可用为`None`；
- `__annotations__`是包含在类主体执行期间收集的变量注释的字典。

<br>

**Class instances**
通过调用类对象来创建类实例。类实例具有作为字典实现的名称空间，该字典是搜索属性引用的第一个位置。当在那找不到属性，并且实例的类具有该名称的属性时，搜索继续使用类属性。如果找到的类属性是用户定义函数对象，则将其转换为实例方法对象，其`__self__`属性为实例。静态方法和类方法对象也同样被转换。如果没有找类属性，并且对象的类具有`__getattr__()`方法，则调用该方法以满足查找。

属性分配和删除更新实例的字典，而不是类的字典。如果类具有`__setattr__()`或`__delattr__()`方法，则调用此方法而不是直接更新实例字典。
如果类实例具有某些特殊名称的方法，则它们可以假装为数字，序列或映射。

特殊属性：

- `__dict__`是属性字典；
- `__class__`是实例的类。

<br>

**I/O objects (also known as file objects)**
文件对象表示打开的文件。各种快捷方式可用于创建文件对象：`open()`內建函数，`os.popen()`，`os.fdopen()`，套接字对象的`makefile()`方法。

`sys.stdin`, `sys.stdout`和`sys.stderr`对象被初始化为与解释器的标准输入，标准输出和标准错误流相对应的文件对象。它们都以文本模式打开，因此遵循`io.TextIOBase`抽象类定义的接口。

<br>

**Internal types**
解释器内部使用的一些类型向用户公开。它们的定义可能会随着解释器的未来版本而改变，但为了完整起见，这里提到它们。

- **Code objects**
代码对象表示字节编译的可执行Python代码或字节码。代码对象和函数对象之间的区别在于函数对象包含对函数的全局变量的显示引用，而代码对象不包含上下文；默认参数值 也存储在函数对象中，而不是存储在代码对象中。与函数对象不同，代码对象是不可变的，并且不包含可变对象的引用。
特殊的只读属性：
- `co_name`给出函数名；
- `co_argcount`是位置参数的数量；
- `co_nlocals`是函数使用的局部变量数；
- `co_varnames`是包含局部变量名称的元组；
- `co_cellvars`是包含嵌套函数引用的局部变量的名称；
- `co_freevars`是包含自由变量名称的元组；
- `co_consts`包含字节码使用文字的元组；
- `co_names`包含字节码使用名称的元组；
- `co_filename`是编译代码的文件名；
- `co_firstlineno`是函数的第一个行号；
- `co_lnotab`是用于编码从字节码偏移到行号的映射；
- `co_stacksize`是必须的堆栈大小；
- `co_flags`是一个整数，用于编码解释器的许多标志；

如果代码对象表示韩式，则`co_consts`中的第一项是函数的文档字符串，如果未定义，则为`None`。

- **Frame objects**
帧对象表示执行帧。它们可能出现在回溯(traceback)对象中。
特殊只读属性:
  - `f_back`是前一个堆栈帧，如果这是底部堆栈帧(bottom stack frame)，则为`None`；
  - `f_code`是在此帧中执行的代码对象；
  - `f_locals`是用于查找局部变量的字典；
  - `f_global`用于全局变量；
  - `f_builtins`用于內建名称；
  - `f_lasti`给出了精确的指令。
特殊可写属性:
  - `f_trace`，如果非`None`，则在每个源代码行的开头调用的函数；
  - `f_lineno`是帧的当前行号——从跟踪函数内写入此跳转到给定行。
帧对象支持一个方法:
  - `frame.clear()`
  此方法清除帧所持有的局部变量的所有引用。如果帧属于生成器，则确定生成器。这有助于打破涉及帧对象的参考循环。
  如果帧当前正在运行，则引发`RuntimeError`。

- **Traceback objects**
回溯对象表示异常的堆栈追踪，发生异常时会创建回溯对象。当搜索异常处理程序展开执行堆栈时，在每个展开的级别上，会在当前回溯之前插入回溯对象。输入异常处理程序时，堆栈追踪可供程序使用。它可作为`sys.exc_info()`返回的元组的第三项进行访问。当程序不包含合适的处理程序时，堆栈追踪被写入到标准错误流；如果解释器是交互式的，那它可作为`sys.last_traceback`提供给用户。
特殊只读属性:
  - `tb_next`是堆栈追踪中的下一级，如果没有，则为`None`；
  - `tb_frame`指向当前级别的执行帧；
  - `tb_lineno`给出发生异常的行号；
  - `tb_lasti`表示准确的指令。
如果在没有匹配的`except`子句或`finally`子句的`try`语句中发生异常，则回溯中的行号和最有一条指令可能与其帧对象的行号不同。

- **Slice objects**
切片对象用于表示`__getitem__()`方法的切片。它们也是由内置的`slice()`函数创建的。
特殊只读属性:
  - `start`是下限；
  - `stop`是上限；
  - `step`是步长；
  - 如果忽略，则每个都是`None`。
切片对象支持一个方法:
  - `slice.indices(self, length)`
  此方法采用单个整数参数长度，并计算切片对象在应用于一些列长度项时将描述的切片的信息。它返回一个由三个整数组成的元组——`(start, stop, step)`。以和常规切片一致的方法处理丢失或越界的索引。

- **Static method objects**
静态方法对象提供了一种破坏函数对象到上述方法对象的转换的方法。静态方法对象是任何其它对象的包装器，通常是用户定义的方法对象。当从类或类实例中检索静态方法对象时，实际返回的对象时包装的对象，该对象不受任何进一步转换的影响。静态方法对象本身不可调用，尽管它们通常包装的对象是。静态方法对象由內建的`staticmethod()`够赞函数创建。

- **Class method objects**
类方法对象，与静态方法对象类似，是另一个对象的包装器，它改变了从类和类实例中检索该对象的方式。类方法对象由內建的`classmethod()`构造函数创建。



<br/>
<br/>
<br/>



### 特殊方法名称

Special method names

类可以通过定义具有特殊名称的方法来实现有特殊语法调用的某些操作。这是Python的运算符重载方法，允许类根据语言运算符定义自己的行为。
将特殊方法设置为`None`表示相应的操作不可用。例如，如果将类的`__iter__()`设置为`None`，则该类不可迭代，因此在其实例调用`iter()`将引发`TypeError`。


<br/>


#### 基本定制

Basic customization

- `object.__new__(cls[,...])`
被调用来创建类`cls.__new__()`的新实例是一个静态方法，它将请求实例的类作为其第一个参数。其余参数是传递给对象构造函数表达式的参数。























<br/>
<br/>
<br/>
<br/>





## 执行模块

[Execution model](https://docs.python.org/3.6/reference/executionmodel.html)


<br/>


### 程序结构

Structure of a program


Python程序由代码块构成。块是一段Python程序文档，作为一个单元执行。模块、函数体、类定义都是块。交互式输入的每个命令都是一个块。脚本文件是代码块，脚本命令是代码块。传递给内建函数`eval()`和`exec()`的字符串参数是一个代码块。



<br/>
<br/>



### 命名和绑定

Naming and binding


<br/>


#### 名称绑定

Binding of names

名称指的是对象。名称有名称绑定操作引入。

以下构造绑定名称:
- 函数的形式参数(formal parameters)
- `import`语句
`from...import *`的`import`语句绑定导入模块中定义的所有名称，但以下划线(`_`)开头的名称除外。
- 类和函数的定义
- 作为标识符的目标
- `for`循环头
- 在`with...as`或`except`子句之后

在`del`语句中出现的目标也被认为是此目的绑定的。
每个赋值或导入语句都发生在由类或函数定义或模块级别定义的块中。
如果名称绑定在块中，则它是该块的局部变量(local variable)，除非声明为`nonlocal`或`global`。如果名称绑定在模块级别，则它是全局变量(global variable)。模块代码块中的变量是本地和全局的。如果一个变量在代码块中使用但未在此定义，则它是一个自由变量(free variable)。
程序文本中每次出现的名称都是指由以下名称解析规则建立的名称的绑定。


<br/>
<br/>


#### 名称解析

Resolution of names

范围(scopt)定义了块内名称的可见性。如果在块中定义了局部变量，则其范围包括该块。如果定义发生在函数块内，则作用于将扩展到定义块中包含的任何块，除非包含块为名称引入不同的绑定。

在代码块中使用名称时，将使用最近的封闭范围解析该名称。代码块可见的所有此类范围的集合称为块的环境。
如果找不到名称，则会引发`NameError`异常。如果当前作用域是函数作用域，并且名称引用尚未绑定到使用该名称的值的本地变量，则会引发`UnboundLocalError`异常(它是NameError的子类)。

如果名称绑定操作发生在代码块中的任何位置，则块中名称的所有使用都将被视为对当前块的引用。在绑定之前在块中使用名称时，可能会导致错误。这条规则很微妙，Python缺少声明，并允许在代码块中的任何位置进行名称绑定操作。可以通过扫描块的整个文本以确定名称绑定操作来确定代码块的局部变量。

如果`global`语句发生在块中，则语句中指定的名称的所有使用都将引用在顶级命名空间中的绑定的名称。通过搜索全局(global)命名空间(包含代码块的命名空间)和內建(build-in)命名空间(內建模块的命名空间)，在顶级命名空间中解析名称。首先搜索全局命名空间。如果在那里找不到名称，则搜索內建命名空间。`global`语句必须在名称的所有使用之前。

`global`语句与同一块中名称绑定具有相同的范围。如果自由变量的最近封闭范围包含全局语句，则将自由变量视为全局变量。
`nonlocal`语句使相应的名称引用最近的封闭函数范围中的先前绑定的变量。如果任何封闭的函数作用域中不存在给定的名称，则在编译时引发`SyntaxError`。

模块的命名空间在第一次导入模块式自动创建。脚本的主要模块始终被称为`__main__`。

`exec()`和`eval()`的类定义块和参数在名称解析的上下文中是特殊的。类定义是可以使用和定义名称的可执行语句。这些引用遵循名称解析的常规规则，但在全局命名空间中查找未绑定的局部变量。类定义的命名空间成为类的属性字典。类块中定义的名称范围仅限于类块;它没有扩展到方法的代码块——这包括了解和生成器表达式，因为它们是使用函数作用域实现的。


<br/>
<br/>


#### 內建和严格执行

Builtins and restricted execution

CPython实现细节：用户不该触碰`__builtins__`，它严格来说是一个实现细节。想要覆盖內建命名空间中的值的用户应该导入內建模块并适当地修改其属性。
与代码块执行相关联的內建命名空间实际上是通过在其全局命名空间中查找名称`__builtins__`来找到的；这应该是字典或模块。默认情况下，在`__main__`模块中，`__builtin__`是內建的`builtins`模块；在任何其它模块中，`__builtins__`是內建模块本身的字典的别名。


<br/>
<br/>


#### 与动态功能的交互

 Interaction with dynamic features

自由变量的名称解析在运行时发生，而不是在编译时发生。

`eval()`和`exec()`函数无权访问用于解析名称的完整环境。可在调用者的本地好全局命名空间中解析名称。自由变量不在最近的封闭空间中解析，而是在全局命名空间中解析。



<br/>
<br/>
<br/>



### 异常

Exceptions

**异常(exception)**是一种打破代码块正常控制流已处理错误或其它异常情况的方法。检测到错误时会出现异常，它可以由周围的代码块直接或间接调用发生错误的代码块的任何代码块处理。

Python解释器在检测到运行时错误引发异常。Python程序也可使用`raise`语句显式地引发异常。使用`try...execpt`语句处理异常，这种语句的`finally`子句可用于指定不处理异常的清理代码。
Python使用错误处理的`termination`模型：异常处理程序可找出发生的情况并继续在外层执行，但它无法修复错误原因并重试失败操作。

当根本不处理异常时，解释器终止程序的执行，或返回其交互式主循环。在任何一种情况下，它都会打印stack backtrace除非异常是`SystemExit`。
异常由类实例标识。根据实例的类选择`except`子句，它必须引用实例的类或其基类。该实例可以由处理程序接收，并且可以携带关于异常情况的附加信息。




<br/>
<br/>
<br/>
<br/>




## import系统

[The import system](https://docs.python.org/3.6/reference/import.html)


通过**导入(import)**，一个模块中的Python代码可以访问另一个模块中的Python代码。`import`语句是调用导入机制的最常用方法，但它不是唯一的方法。`importlib.import_module()`和內建`__import__()`等函数也可用于导入机制。

`import`语句结合了两个操作，它搜索命名模块，然后将搜索结果绑定到本地范围中的名称。`import`语句的搜索操作定义为使用适当的参数调用`__import__()`函数。`__import__()`的返回值用于执行`import`语句的名称绑定操作。

对`__import__()`的直接调用仅执行模块搜索，如果找到，则执行模块创建操作。虽然可能会发生某些副作用，如导入父包...但只有`import`语句执行名称绑定操作。

当调用`__import__()`作为`import`语句的一部分时，将调用标准的内置`__import__()`。调用导入系统的其它机制可选择颠覆`__import__()`并使用自己的解决方案来实现导入语义。

首次导入模块时，Python会搜索模块。如果找到，它会创建一个模块对象，并对其进行初始化。如果找不到名称模块，则引发`ModuleNotFoundError`异常(No module named 'xxx')。Python在调用导入机制时实现了搜索命名模块的各种策略，可以修改和扩展这些策略。


<br/>
<br/>


### importlib

`importlib`模块提供了一个丰富的API，用于与导入系统进行交互。



<br/>
<br/>



### Packages

Python只有一种类型的模块对象，所有模块都属于这种类型，无论模块是用Python、C还是其它方式实现。为了帮助组织模块并提供命名层次结构，Python有一个**包(package)**的概念。

你可将包视为文件系统上的目录，将模块视为目录中的文件。处于此文档的目的，我们将使用这种方便的类比。包是按层次结构组织的，包本身可能包含子包以及常规模块。
需要记住的是，所有包都是模块，但并非所有模块都是包。换句话说，包是一种特殊的模块。具体来说，任何包含`__path__`属性的模块都被视为包。

Python定义了两种类型的包：

- Regular Packages
- Namespace packages


<br/>
<br/>


#### Regular packages

常规包是Python v3.2及更早版本中存在的传统的包。常规包通常实现为包含`__init__.py`文件的目录。导入常规包时，将隐式地执行此`__init__.py`文件，并且它定义的对象将绑定到包命名空间的名称。`__init__.py`文件可以包含与任何其它模块可以包含的相同的Python代码，并且Python将在导入模块时向模块添加一些其它属性。

```
# 栗子
# 导入parent.one将隐式地执行parent/__init__.py和parent/one/__init__.py
parent/
    __init__.py
    one/
        __init__.py
    two/
        __init__.py
    three/
        __init__.py
```


<br/>
<br/>


#### Namespace packages

命名空间包是各个部分的组合，其中每个部分为父包提供子包。它可位于文件系统某个位置上，也可在zip文件、网络或Python导入期间其它位置。命名空间包可能/可能不(may or may not)直接对应于系统上得对象，它们可能是没有具体表示的虚拟模块。

命名空间包不使用普通列表作为其`__path__`属性。它们使用自定义可迭代类型，如果其父包的路径发生更改，它将在该包的下一次导入尝试时自动执行对包部分的新搜索。

使用命名空间包，没有`parent/__init__.py`文件。实际上，在导入搜索期间可能会找到多个父目录，其中每个目录由不同的部分提供。因此，`parent/one`可能不在物理上位于`parent/two`旁边。在这种情况下，Python就会为顶级父包创建一个命名空间包。



<br/>
<br/>



### Searching

要开始**搜素(search)**，Python需要导入模块的完全限定名称。此名称可能来自`import`语句或其它导入函数的各种参数。


<br/>
<br/>


#### 模块缓存

The module cache

导入期间检查的第一个位置是`sys.modules`。此映射用作先前已导入的所有模块的缓存，包括中间路径。因此，如果先前导入了`a.b.c`，则`sys.modules`将包含`a, a.b, a.b.c`的条目。每个键的值都是相应的模块对象。

在导入期间，将在`sys.modules`中查找模块名称。如果存在，则关联的值是满足导入的模块，并且该过程完成。但是，如果值为`None`，则引发`ModuleNotFoundError`异常。如果缺少模块名称，Python将继续搜索模块。

`sys.modules`是可写的。删除key可能不会破坏关联的模块，但它会使命名模块的缓存条目无效，导致Python在下次导入时重新搜索命名模块。key也可以分配`None`，强制下次导入模块导致`ModuleNotFoundError`异常。

请注意，就好像你保留对模块对象的引用，使其在`sys.modules`中的缓存条目无效，然后重新导入命名模块，两个模块对象将不同。相比之下，`importlib.reload()`将重用相同的模块对象，只需重新运行模块的代码即可重新初始化模块内容。


<br/>
<br/>


#### 查找器和载入器

Finders and loaders

如果在`sys.modules`中找不到指定的模块，则调用Python的导入协议来**查找(find)**和**加载(load)**模块。该协议有两个概念对象。实现这两个接口的对象称为导入器，当它发现可以加载所请求的模块时，它们会自行返回。

- **查找器(finder)**：查找器的工作是确定它是否可以使用它所知道的任何策略找到命名模块。
- **加载器(loader)**

Python包含了许多默认查找器和导入器。第一个知道如何定位內建模块，第二个知道如何定位冻结模块。第三个默认查找器搜索模块的导入路径。
导入机制是可扩展的，因此可以添加新的查找器以扩展模块搜索的范围。

查找器实际上并没有加载模块。如果它可以找到命名模块，则返回模块规范、模块的导入相关信息的封装，然后导入机器在加载模块时使用。


<br/>
<br/>


#### Import hooks

导入机制设计为可扩展，它的主要机制是**导入钩子(import hooks)**。它有两种类型：

- **Meta hooks**：在进行任何其它导入之前，在导入处理开始时调用元钩子，而不是使用`sys.modules`缓存查找。这允许云钩子覆盖`sys.path`、冻结模块甚至内置模块。
- **Path hooks**：导入路径钩子作为`sys.path`处理的一部分在遇到其关联路径时被调用。



<br/>
<br/>



### 载入

Loading

如果找到模块规范，导入机制将在加载模块时使用它。

```python
module = None
if spec.loader is not None and hasattr(spec.loader, 'create_module'):
    # It is assumed 'exec_module' will also be defined on the loader.
    module = spec.loader.create_module(spec)
if module is None:
    module = ModuleType(spec.name)
# The import-related module attributes get set here:
_init_module_attrs(spec, module)

if spec.loader is None:
    if spec.submodule_search_locations is not None:
        # namespace package
        sys.modules[spec.name] = module
    else:
        # unsupported
        raise ImportError
elif not hasattr(spec.loader, 'exec_module'):
    module = spec.loader.load_module(spec.name)
    # Set __loader__ and __package__ if missing.
else:
    sys.modules[spec.name] = module
    try:
        spec.loader.exec_module(module)
    except BaseException:
        try:
            del sys.modules[spec.name]
        except KeyError:
            pass
        raise
return sys.modules[spec.name]
```


























<br>
<br/>

---

<br>



# HOWTOs


Python HOWTOs是覆盖单个特定主机的文档，并尝试完全包含它。此文档比Python参考库更详细。






<br>
<br/>

---

<br>



# 标准库



<br>
<br/>



## 介绍


Python标准库包含了各种不同类型的组件。

一些模块提供了特定于Python的接口；一些提供特定于特定操作系统的接口，一些提供特定于特定应用程序的接口。
一些模块适用于所有Python版本和端口；一些只有在底层系统支持或需要它们是才可用；还有一些只有在编译和安装Python特定配置时才可用。



<br>
<br>


## 内建函数


Python解释器内置了许多功能和类型，它们始终可用。

| ` ` | ` ` | 内建函数 | ` ` | ` `
| - | - | - | -
| `abs()` | `dict()` | `help()` | `min()` | `setattr()`
| `all()` | `dir()` | `hex()` | `next()` | `slice()`
| `any()` | `divmod()` | `id()` | `object()` | `sorted()`
| `ascii()` | `enumerate()` | `input()` | `oct()` | `staticmethod()`
| `bin()` | `eval()` | `int()` | `open()` | `str()`
| `bool()` | `exec()` | `isinstance()` | `ord()` | `sum()`
| `bytearray()` | `filter()` | `issubclass()` | `pow()` | `super()`
| `bytes()` | `float()` | `iter()` | `print()` | `tuple()`
| `callable()` | `format()` | `len()` | `property()` | `type()`
| `chr()` | `frozenset()` | `list()` | `range()` | `vars()`
| `classmethod()` | `getattr()` | `locals()` | `repr()` | `zip()`
| `compile()` | `globals()` | `map()` | `reversed()` | `__import__()`
| `complex()` | `hasattr()` | `max()` | `round()` | ` `
| `delattr()` | `hash()` | `memoryview()` | `set()` | ` `


<br>

- **abs(x)**
返回一个数字的绝对值

- **all(iterable)**
如果迭代的所有元素均为真(或为空)，返回`True`

- **any(iterable)**
如果迭代的任一元素为真，返回`True`；为空返回`False`

- **ascii(object)**

- **bin(x)**
将整数转换为二进制字符串

- **bool([x])**
返回一个布尔值，`True`或`False`

- **bytearray()**
返回一个新的字节数组

- **byte()**
返回一个新的字节对象，它是一个在`0<=x<256`范围内的不可变整数序列

- **callable(object)**
如果对象参数显示为可调用，返回`True`；否则返回`False`

- **chr(i)**
返回代表Unicode编码为整数i的字符的字符串

- **classmethod(function)**
为函数返回一个类方法

- **compile()**
将源编译为代码或AST对象

- **complex()**
返回一个复数，或将字符串或数字转换为复数

- **delattr(object, name)**
这是`setattr()`的相对值

- **dict(kwarg)**
创建一个新的字典

- **dir(object)**
无参数，返回当前本地作用域中的名称列表
有参数，尝试返回该对象的有效属性列表

- **divmod(a, b)**
以两个数字(非复数)为参数，使用整数除法时返回由它们的商和余数组成的一对数字

- **enumerate(iterable, start=0)**
返回一个枚举对象

- **eval(expression, globals, locals)**

- **exec()**
动态执行Python代码

- **filter(function, iterable)**
从函数返回true的可迭代元素构造一个迭代器

- **float()**
返回由数字或字符串构造的浮点数

- **format()**
将值转换为特定格式

- **frozenset()**
返回一个新的`frozenset`对象，可选用来自迭代的元素

- **getattr()**
返回对象命名属性的值

- **globals()**
返回表示当前全局符号表的字典

- **hasattr(obj, name)**
参数是一个对象和一份字符串，如果字符串是对象属性之一的名称，结果为`True`，否则`False`

- **hash(obj)**
返回对象的hash值

- **help()**
调用内建的帮助系统

- **hex(x)**
将整数转换为十六进制数

- **id(obj)**
返回一个对象的标识

- **input()**
从标准输入中读取一行，转换为字符串，然后返回该行

- **int(x)**
返回一个整数对象，如果没有参数，则返回0

- **isinstance(obj, classinfo)**
如果对象参数是classinfo参数的实例或其子类的实例，返回true

- **issubclass(class, classinfo)**
如果class是类信息的子类，返回true

- **iter(obj)**
返回一个迭代器对象

- **len()**
返回对象的长度

- **list()**
列表实际上是一个可变的序列类型，而不是一个函数

- **locals()**
更新并返回表示当前本地符号表的字典

- **map()**
返回一个将函数应用于每个迭代项的迭代器，从而产生结果

- **max()**
返回最大项

- **memoryview(obj)**
从给定参数返回内存视图对象

- **min()**
返回最小项

- **next()**
从迭代器中检索下一项

- **object()**
返回一个新的无特征的对象

- **oct()**
将整数转换为八进制字符串

- **open()**
打开文件并返回相应的文件对象

- **ord()**
给定一个表示一个Unicode编码的字符，返回一个表示该字符的Unicode编码的整数

- **pow()**

- **print()**
将对象打印到流文件

- **property()**
返回一个property属性

- **range()**
范围一个不可变的序列类型，而不是函数

- **repr()**
返回一个包含对象可打印表示的字符串

- **reversed()**
返回一个反向迭代器

- **round()**
返回数字小数点后ndigits精度

- **set()**
返回一个新的集合对象，可选来自迭代的元素

- **setattr()**
`getattr`的对应部分

- **slice()**
返回由范围指定的一组索引的切片(slice)对象

- **sorted()**
从迭代项中返回一个新的排序列表

- **staticmethod()**
为函数返回一个静态方法

- **str()**
返回一个字符串对象

- **sum()**
对迭代项求和

- **super()**
返回将方法调用委托个父类或同类的代理对象

- **tuple()**
元组是一个不可变的序列类型，而不是函数

- **vars()**
返回对象的`__dict__`属性

- **zip()**
制作一个迭代器，用于聚合来自每个迭代器的元素

- `__import__`
这个函数被`import`语句调用



<br>
<br/>


## 内建常量


少量常量存在于命名空间中。

- **False**
- **True**
- **None**
- **NotImplemented**
- **Ellipsis**
- `__debug__`



<br>
<br/>



## 内建类型


主要的内建类型有：

- 数字(numeric)
- 序列(sequence)
- 映射(mapping)
- 类(class)
- 实例(instance)
- 异常(exception)


<br>
<br/>


### 真值测试

任何对象都可进行真值测试。


<br>
<br/>


### 布尔操作


- and
- or
- not


<br>
<br/>


### 比较操作


Python中有8个比较操作：

- `<`
- `<=`
- `>`
- `>=`
- `==`
- `!=`
- `is`
- `isnot`


<br>
<br/>


### 数字类型


- int
- float
- complex


<br>
<br/>


### 迭代器类型


Python支持对容器进行迭代的概念。


<br>
<br/>


### 序列类型


- list
	+ 列表是可变序列，通常用于存储同类项目的集合
- tuple
	+ 元组是不可变序列，通常用于存储异构数据的集合
- range
	+ 范围表示一个不可变的数字序列，通常用于`for`循环
	+ range(start, stop, step)


<br>

#### 通用序列操作

- `in`
- `not in`
- `+`
- `*`
- `[i]`
- `[i:j]`
- `[i:j:k]`
- `len()`
- `min()`
- `max()`
- `count()`
- `index()`

<br>

#### 不可变序列类型

<br>

#### 可变序列类型

可变定义类型的操作：

- `[i]`
- `[i:j]`
- `del [i:j:k]`
- `append()`
- `clear()`
- `copy()`
- `+=`
- `*=`
- `insert()`
- `pop`
- `remove()`
- `reverse()`


<br>
<br/>


### 文本序列类型


Python中的文本数据由str对象处理，字符串是Unicode编码点的不可变序列。

字符串以各种方式书写：

- 单引号
- 双引号
- 三引号

<br>

#### 字符串方法

- <https://docs.python.org/3.5/library/stdtypes.html#string-methods>

<br>

#### 样式字符串格式

字符串对象有一个唯一的内建操作: `%`操作符，也称为字符串格式化操作符。

转换类型：

- `%`
- `s`
- `i`
- `x`
- `f`
- `c`


<br>
<br/>


### 二进制序列类型


- bytes
	+ 字节对象是单字节的不可变序列
- bytearray
	+ 是字节对象的可变对象
- memoryview
	+ 运行Python代码访问支持缓冲区协议的对象的内部数据，而无需复制

字节和字节数组对象操作符都支持普通序列操作符，同样也支持字节格式。


<br>
<br/>


### 集合类型


- set
- frozenset

集合对象是不同可散列对象的无序集合。常见用途包含成员测试、删除重复项，数学计算(交集，并集，差集)


<br>
<br/>


### 映射类型

- dict

映射对象可将散列值映射到任意对象，它是可变对象。

<br>

#### 字典视图对象

- `dict.keys()`
- `dict.values()`
- `dict.items()`


<br>
<br/>


### 上下文管理类型


Python的`with`语句支持由上下文管理器定义的运行时上下文的概念。


<br>
<br/>


### 其它内建类型


#### 模块

#### 类和类实例

#### 函数

#### 方法

#### 代码对象

#### 类型对象

#### null对象

#### ellipsis对象

#### notImplimented对象

#### 布尔值

#### 内部对象


<br>
<br/>


### 特殊属性


一些特殊的只读属性：

- `object.__dict__`
- `instance.__class__`
- `class.__bases__`
- `definition.__name__`
- `definition.__qualname__`
- `class.__mro__`
- `class.__subclasses__()`



<br>
<br/>



## 内建异常


在Python中，所有异常(exception)都必须是派生自Baseexception的类的实例。


<br>
<br/>


### 基类


- BaseException
- Exception
- ArithmeticError
- bufferError
- LookupError


<br>
<br/>


### 具体异常


- AssertionError
- AttributeError
- EOFError
- FloatingPointError
- GeneratorExit
- ImportError
- IndexError
- KeyError
- KerboardInterrupt
- MemoryError
- NameError
- NotImplementedError
- OSError
- OverflowError
- RecursionError
- ReferenceError
- RuntimeError
- StopAsyncIteration
- SyntaxError
- IndentationError
- TabError
- SystemError
- SystemExit
- TypeError
- UnboundLocalError
- UnicodeError
- UnicodeEncodeError
- UnicodeDecodeError
- UnicodeTranslateError
- ValueError
- ZeroDivisionError
- EnvironmentError
- IOError


<br>
<br/>



## 文本处理


### string


`string`模块，字符串操作


<br>
<br>


### re


`re`模块，提供了正则表达式匹配操作。

**字符串模式匹配**

```py
>>> import re
>>> re.findall(r'f[a-z]*', 'which foot or hand fell fastest')
['foot', 'fell', 'fastest']


#替换
>>> 'aaa and bbb'.replace('bbb', 'BBB')
'aaa and BBB'
```



<br>
<br/>


### difflib

difflib，助手计算三角洲。
该模块提供用于比较序列的类和函数。


<br>
<br>

### textwrap

`textwrap`模块，文本环绕和填充。
将段落文本格式化以适应给定的屏幕宽度。

```py
>>> import textwrap
>>> doc = """ 1111 1111 1111 1111 1111 1111
... 2222 2222 2222 2222 2222 2222
... 3333 3333 3333 3333 3333 3333"""

>>> print(textwrap.fill(doc, width=50))
 1111 1111 1111 1111 1111 1111 2222 2222 2222 2222
2222 2222 3333 3333 3333 3333 3333 3333
```



<br>
<br/>


### unicodedata


unicodedata，Unicode数据库。
该模块提供对Unicode字符数据库(UCD)的访问，此数据库为所有Unicode字符定义字符属性。


<br>
<br/>


### stringprep


stringprep，因特网字符串准备。


<br>
<br/>


### readline


readline，GNU读行接口。
该模块定义了许多方便Python解释器完成和读写历史文件的函数。


<br>
<br/>


### rlcompleter

rlcompleter，GNU读行的完成函数。
该模块通过完成有效的Python标识符合关键字来定义适用于readline模块的完成函数。



<br>
<br/>


## 二进制数据


### struct

`struct`模块，将字节解释为打包的二进制数据。
提供了`pack()`和`unpack()`函数来处理可变长度的二进制记录格式。


<br>
<br/>


### codecs

codes，编解码注册和基类。



<br>
<br/>



## 数据和时间


### time

`time`模块，提供了许多操作时间值(time value)的函数，用于取得Unix纪元时间戳。

```py
import time

#Unix时间
time.time()
#1531364576.3187952

#delay for a number of seconds given as a float
time.sleep()
time.time();time.sleep(10);time.time()


```


<br/>
<br/>


### datetime

`datetime`模块，基本日期和时间类型。
支持日期和时间计算，并对输出做格式化处理。

```py
import datetime

datetime.datetime.now()
#datetime.datetime(2018, 7, 12, 13, 45, 43, 127838)
datetime.datetime.now().year, datetime.datetime.now().month, datetime.datetime.now().hour

#Unix纪元转换
datetime.datetime.fromtimestamp(1531374507.8268566)
#datetime.datetime.fromtimestamp(time.time())
#datetime.datetime(2018, 7, 12, 13, 48, 27, 826857)


#日期比较
yesterday = datetime.datetime(2018, 7, 11, 00, 00, 00, 00000)
today = datetime.datetime.now()
future = datetime.datetime(2018, 8, 12, 00, 00, 00, 00000)
today > yesterday
while future > today:
    time.sleep(1)


#timedelta表示一段时间
#周，时，分，秒，毫秒，微秒
period = datetime.timedelta(days=7, hours=6, minutes=20, seconds=55)
str(period)
#'7 days, 6:20:55'

```

`datetime.datetime.strftime()`将`datetime`对象转换为字符串
`datetime.datetime.strptime()`将字符串转换为`datetime`对象
格式栗子:

- `%Y`: 2018
- `%y`: 18
- `%m`: 07
- `%B`: July
- `%b`: Jul
- `%d`: 一月中的第几天
- `%j`: 一年中的第几天
- `%w`: 一周中的第几天(0-6)
- `%A`: Thursday
- `%a`: Thu
- `%H`: 14(00-23)
- `%I`: 2(0-12)
- `%M`: 分(00-59)
- `%S`: 秒(00-59)
- `%p`: AM/PM

```py
datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
#'2018-07-12 14:11:20'

datetime.datetime.strptime(2018-07-12 14:11:20', '%Y-%m-%d %H:%M:%S')
#datetime.datetime(2018, 7, 12, 14, 11, 20)

```


<br>
<br/>


### calendar

calendar，日历相关函数。


<br>
<br/>


### collections

collections，容器数据类型。


<br>
<br/>


### collections.abc

collections.abc，容器的抽象基类


<br>
<br/>


### heapq

heapq，堆队列算法。


<br>
<br/>


### bisect

bisect，数组二等分算法。


<br>
<br/>


### array

`array`模块，有效的数值数组。
它提供了一个`array()`对象，就像一个只存储同质数据并将其存储更紧凑的列表。


<br>
<br/>


### weakref

weakref，弱引用。
weakref模块提供了用于跟踪对象而不创建参考的工具。当不再需要该对象时，它将自动从弱参考表中移除，并为弱参考对象触发回调。

Python支持自动内存管理，内存在最后一次被删除后不久就被释放。


<br>
<br/>


### copy

copy，浅层和深层操作。


<br>
<br/>


### pprint

`pprint`模块，漂亮打印。

```py
>>> import pprint
>>> t =[[[['black', 'cyan'], 'white', ['green', 'red']], [['magenta','yellow'], 'blue']]]
>>> pprint.pprint(t, width=30)
[[[['black', 'cyan'],
   'white',
   ['green', 'red']],
  [['magenta', 'yellow'],
   'blue']]]
```




<br>
<br/>


### reprlib

`reprlib`模块，提供repr自定义显示


<br>
<br/>


### enum

enum，支持枚举。



<br>
<br/>


## 数字和数学


### numbers

numbers，数字抽象基类。


<br>
<br/>


### round

按指定精度四舍五入一个浮点数。

```
round(1.23456, 4)
#1.236

```


<br/>
<br/>



### math

`math`模块，数学函数。

```py
>>> import math
>>> math.sin(math.pi / 2)
1.0
>>> math.log(256, 2)
8.0
```


<br>
<br/>


### cmath

cmath，复数数学函数。


<br>
<br/>


### decimal

decimal，十进制定点和浮点运算。


<br>
<br/>


### fractions

fractions，有理数。


<br>
<br/>


### random

random，生成伪随机数。

```py
>>> import random
>>> random.choice(['a', 'b', 'c'])
'a'
>>> random.sample(range(10), 2)
[2, 6]
>>> random.random()
0.9714711378164909
>>> random.randrange(10)
5
```


<br>
<br/>


### statistics

`statistics`模块，数学统计函数。
计算基本的统计属性：
 - 平均数(mean)
 - 中位数(median)
 - 方差(variance)

```py
>>> import statistics
>>> num = [1, 2, 3, 4, 5]
>>> statistics.mean(num)
3
>>> statistics.median(num)
3
>>> statistics.variance(num)
2.5
```


<br>
<br/>



## 函数式编程模块


本章模块提供了支持函数式编程风格的函数和类，以及可调用函数的一般操作。

<br>
<br/>


### itertools

itertools，为高校循环创建迭代器。


<br>
<br/>


### functools

functools，可调用对象的高阶函数和操作


<br>
<br/>


### operator

operator，作为函数的标准操作符



<br>
<br/>


## 文件和目录

本章介绍的模块处理磁盘文件和目录。


<br>
<br/>


### pathlib

pathlib，面向对象的文件系统路径
此模块提供了代表文件系统路径的类，其语义适用于不同的操作系统。


<br>
<br/>


### os.path

os.path，通用路径名操作
该模块在路径名上实现了一些有用的功能。


<br>
<br/>


### fileinput

fileinput，迭代来自多个输入流的行
该模块实现了从一个帮助类和函数，可在标准输入或文件列表上快速编写循环。


<br>
<br/>


### stat

stat，解释`stat()`结果
此模块定义用于解释`os.stat()`,`os.fstat()`,`os.lstat()`的结果的常量和函数。


<br>
<br/>


### filecmp

filecmp，文件和目录比较
此模块定义了比较文件和目录的函数，以及各种可选的时间和权衡。


<br>
<br/>


### tempfile

tempfile，生成临时文件和目录
此模块创建临时文件和目录。


<br>
<br/>


### glob

glob，Unix样式路径名称模式扩展
此模块根据Unix shell使用的规则查找与指定模式匹配的所有路径名，结果以任意顺序返回。


<br>
<br/>


### fnmatch


fnmatch，Unix文件名模式匹配
此模块提供了对Unix shell风格的通配符的支持，它与正则表达式不同。

通配符:

- `*`
- `?`
- `[seq]`
- `[!seq]`


<br>
<br/>


### linecache

linecache，随机访问文本行
此模块允许从Python源文件中获取任意行，同时尝试使用缓存进行内部优化，这是一种从单个文件中读取多行的常见情况。


<br>
<br/>


### shutil

shutil，高级文件操作
此模块提供了许多关于文件和文件集合的高级操作。

- 目录和文件操作
	+ copytree
	+ rmtree
- 归档操作

```py
>>> shutil.copyfile('/tmp/1.txt', '/tmp/111.txt')
'/tmp/111.txt'

>>> shutil.move('/tmp/today', '/tmp/TODAY')
'/tmp/TODAY
```


<br/>
<br/>


### glob

`glob`模块，从通配符中搜索创建文件列表

```py
>>> import glob
>>> glob.glob('/tmp/*.txt')
['/tmp/1.txt', '/tmp/2.txt', '/tmp/111.txt']
```



<br>
<br/>


## 数据持久化

本章介绍的模块支持将Python数据持久化存储到磁盘上。


<br>
<br/>


### pickle

pickle，Python对象序列化
此模块用于实现序列化(serializing)和反序列化Python对象结构的二进制协议。


<br>
<br/>


### copyreg

copyreg，注册pickle支持函数
该模块提供了一种定义胭脂(pickle)特定对象时使用的函数方法。


<br>
<br/>


### shelve

shelve，Python对象持久化
shelf是一个持久的，类似字典的对象。


<br>
<br/>


### marshal

marshal，内部Python对象序列化
此模块包含了可以以二进制格式读写Python值得函数。


<br>
<br/>


### dbm

dbm，到Unix数据库的接口
dbm是DBM数据库变体的通用接口。


<br>
<br/>


### sqlite3

sqlite3，SQLite数据库的DB-API 2.0接口
SQLite是一个C库，它提供了一个轻量级的基于磁盘的数据库，它不需要单独的服务器进程，并允许使用SQL查询语言的非标准变体访问数据库。


<br>
<br/>


## 数据压缩和归档


本章介绍的模块，支持使用zlib, gzip, bzip2, lzma算法进行数据压缩，以及创建zip和tar格式的归档文件。


<br>
<br/>


### zlib

zlib，兼容gzip的压缩
对于需要数据压缩的应用程序，此模块中的功能允许使用zlib库进行压缩(compression)和解压缩(decompression)。


<br>
<br/>


### gzip

gzip，支持gzip文件
此模块提供了一个简单的接口来压缩和解压缩文件，就行GNU程序gzip和gunzip一样。


<br>
<br>


### bz2

bz2，支持bz2压缩
该模块提供了一个全面的接口，用于使用bzip2压缩算法进行压缩和解压缩数据。


<br>
<br/>


### lzma

lzma，使用lzma算法进行压缩
该模块提供了类和函数，用于使用lzma进行压缩和解压缩数据。


<br>
<br/>


### zipfile

zipfile，使用zip归档
zip文件格式是一个常用的归档和压缩标准。此模块提供了工具，用于创建，读写，追加和列出zip文件的工具。


<br>
<br/>


### tarfile

tarfile，读写tar归档文件
该模块可读写tar归档文件，包括使用gzip，bz2和lzma压缩。


<br>
<br/>


## 文件格式

本章描述的模块，解析各种各样的文件格式，不包含标记语言和e-mail。


<br>
<br/>


### csv

csv，读写CSV文件

所谓的CSV(comma separated values)逗号分隔值，它是一种简化的电子表格，保存为纯文本文件。是电子表格和数据库最常用的导入和导出格式。
该模块实现了以CSV格式读写表格数据。

CSV文件很简单，缺少了Excel表格的许多功能：

- 值没有类型，都是字符串
- 没有字体大小或颜色
- 没有多个工作表
- 不能指定单元格的宽度和高度
- 不能合并单元格
- 不能签入图像和图标

<br>

```
import csv

file = open('/tmp/test.csv')
reader = csv.reader(file)
data = list(reder)

#写
file = open('/tmp/test.csv', 'w', newline='')
writer = csv.writer(file)
writer.writerow('[1, 11, 111]')
file.close()
```


<br/>
<br/>



### configparser

configparser，配置文件解析器
此模块提供了ConfigParser类，它实现了一种基本配置，你可以使用它来编写可由最终用户轻松定制的Python程序。


<br>
<br/>


### netrc

netrc文件处理
netrc类解析和封装Unix FTP程序和其它FTP客户端使用的netrc文件格式。


<br>
<br/>


### xdrlib

xdrlib，编码(encode)和解码(decode)XDR数据
该模块支持外部数据表示标准(External Data Representation Standard)。此模块定义了两个类，一个将变量打包(packing)到XDR，另一个从XDR中解包(unpack)。


<br>
<br/>


## 加密服务

本章描述的模块，实现了各种加密(cryptographic)算法


<br>
<br/>


### hashlib

hashlib，安全散列和消息摘要(digest)
该模块为许多不同安全散列和消息摘要算法实现了通用接口。

- SHA1
- SHA224
- SHA256
- SHA384
- MD5


<br>
<br/>


### hmac

hmac，用于消息认证的键控散列


<br>
<br/>


## 操作系统接口


本章介绍的模块，提供了操作系统功能的接口。


<br>
<br/>


### os

`os`，各种操作系统接口
该模块为使用操作系统相关的功能提供了一种便携方式。

- 文件名，命令行参数，环境变量
- 进程参数
- 文件对象创建
- 文件描述符操作
- 文件和目录的Linux扩展属性
- 进程管理
- 调度程序的接口
- 各种各样的系统信息
- 各种各样的功能

```py
>>> import os
>>> os.getcwd()
'/home/zhang'
>>> os.chdir('/tmp')

#在shell中运行命令
>>> os.system('mkdir /tmp/today')
0
```


<br>
<br/>


### io

io，流处理的核心工具
该模块提供了Python用于处理各种类型I/O的主要工具。

- text i/o
- binary i/o
- raw i/o


<br>
<br/>


### time

time，访问和转换时间
此模块提供了各种与时间相关的函数


<br>
<br/>


### argparse

argparse，解析命令行选项、参数和子命令
该模块可以轻松编写用户友好的命令行接口。


<br>
<br/>


### getopt

getopt，用于命令行选项的C风格解析器
该模块帮助脚本解析`sys.argv`中的命令行参数。



<br>
<br/>



### logging

logging，Python的日志工具。
该模块定义了函数和类，为应用程序和库实现灵活事件记录系统。

<br>

log level:

- `DEBUG`
最低级别。用于小细节，通常只有在诊断问题时，才需要关心这些信息。

- `INFO`
用于记录程序中的一般事件的信息。

- `WARNING`
用于表示可能的问题

- `ERROR`
用于记录错误

- `CRITICAL`
最高级别，用于表示致命的错误

<br>

**日志级别是一种建议。归根到底，还是由你来决定日志消息属于哪一种类型。**

```py
import logging

logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(lineno)d - %(message)s')

logging.debug('Debugging information')
logging.info('Informational message')
logging.warning('Warning:config file %s not found', 'server.conf')
logging.error('Error occurred')
logging.critical('Critical error -- shutting down')


#输出
2018-07-10 14:50:13,060 - INFO - 6 - Informational message
2018-07-10 14:50:13,061 - WARNING - 7 - Warning:config file server.conf not found
2018-07-10 14:50:13,061 - ERROR - 8 - Error occurred
2018-07-10 14:50:13,061 - CRITICAL - 9 - Critical error -- shutting down
```

<br>

日志格式:

```
     |  %(name)s            Name of the logger (logging channel)
     |  %(levelno)s         Numeric logging level for the message (DEBUG, INFO, WARNING, ERROR, CRITICAL)
     |  %(levelname)s       Text logging level for the message ("DEBUG", "INFO", "WARNING", "ERROR", "CRITICAL")
     |  %(pathname)s        Full pathname of the source file where the logging call was issued (if available)
     |  %(filename)s        Filename portion of pathname
     |  %(module)s          Module (name portion of filename)
     |  %(lineno)d          Source line number where the logging call was issued (if available)
     |  %(funcName)s        Function name
     |  %(created)f         Time when the LogRecord was created (time.time() return value)
     |  %(asctime)s         Textual time when the LogRecord was created
     |  %(msecs)d           Millisecond portion of the creation time
     |  %(relativeCreated)d Time in milliseconds when the LogRecord was created, relative to the time the logging module was loaded (typically at application startup time)
     |  %(thread)d          Thread ID (if available)
     |  %(threadName)s      Thread name (if available)
     |  %(process)d         Process ID (if available)
     |  %(message)s         The result of record.getMessage(), computed just as the record is emitted
```



<br>
<br/>


### logging.config

logging.config，日志配置


<br>
<br/>


### logging.handlers

logging.handlers，日志处理程序


<br>
<br/>


### getpass

getpass，便携式密码输入


<br>
<br/>


### curses

curses，字符单元显示的终端处理


<br>
<br/>


### curses.textpad

curses.textpad，用于curses程序的文本输入小部件
此模块提供了一个Textbox类，他在curses窗口中处理基本的文本编辑。


<br>
<br/>


### curses.ascii

curses.ascii，用于ASCII字符的使用程序
该模块为ASCII字符提供名称常量，并为各种ASCII字符类中的成员测试函数。


<br>
<br/>


### curses.panel

curses.panel，curses的面板堆栈扩展
面板是具有深度附加功能的窗口，因此它可堆叠在彼此的顶部，并且只显示每个窗口的可见部分。


<br>
<br/>


### platform

platform，访问底层平台的识别数据


<br>
<br/>


### errno

errno，标准的errno系统符号


<br>
<br/>


### ctypes

ctypes，一个Python的外部函数库
该模块提供了C兼容的数据类型，并允许在DLL或共享中调用函数。


<br>
<br/>


## 并发执行


本章介绍的模块，为并发执行(consurrent execution)代码提供了支持。

这里需要理解一些基本知识:

- **并发**：并发的关键是处理多个任务，不一定要同时，可理解为交替做不同事情；
- **并行**：并行的关键是同时处理多个任务，可理解为同时做不同事情；
- **进程**：进程是操作系统资源分配的最小单元，每个进程拥有独立的内存单元；
- **线程**：线程是程序执行的最小单元，是系统独立调度和分配CPU（独立运行）的基本单位。进程内的线程共享资源。进程内的线程通信比进程之间的通信更快更有效，因为共享资源；
- **多进程**：同时执行多个进程——同时运行wecat, qq
- **多线程**：同时执行多个线程——浏览器边看视频、边听歌、边下载



<br>
<br/>


### threading

threading，基于线程的并行
此模块在较低级别的`_thread`模块之上构建较高级别的线程接口。

如果多线程同时读写变量，导致互相干扰，就会发生所谓的并发问题。

```py
import time, threading

print('thread start.')

def wakeup(times):
    time.sleep(5)
	n = times
	for i in range(n):
        print('Wake Up!')

thread01 = threading.Thread(target=wakeup, args=[3])
#thread01 = threading.Thread(target=wakeup, kwargs={'times': 3})
thread01.start()
print('End of program!')
```

<br>

**使用queue队列通信-经典的生产者和消费者模型**
一个负责生成，一个负责消费，所生成的产品存放在queue里，实现了不同线程间沟通

```python
# Producer
class Producer(threading.Thread):
    def __init__(self, name, queue):
        threading.Thread.__init__(self, name=name)
        self.queue = queue

    def run(self):
        for i in range(1, 5):
            print('{} 生产 {} 到队列'.format(self.getName(), i))
            self.queue.put(i)
            time.sleep(random.randrange(10) / 5)
        print('{} 完成!'.format(self.getName()))

# Consumer
class Consumer(threading.Thread):
    def __init__(self, name, queue):
        threading.Thread.__init__(self, name=name)
        self.queue = queue

    def run(self):
        for i in range(1, 5):
            val = self.queue.get()
            print('{} 消费队列中的 {}'.format(self.getName(), val))
            time.sleep(random.randrange(10))
        print('{} 完成!'.format(self.getName()))

def main():
    queue = Queue()
    producer = Producer('Producer', queue)
    consumer = Consumer('Consumer', queue)
    producer.start()
    consumer.start()
    producer.join()
    consumer.join()
    print('所有线程已完成!')

if __name__ == "__main__":
    main()
```



<br>
<br/>


### multiprocessing

参考: <https://zhuanlan.zhihu.com/p/46368084>

multiprocessing，基于进程的并行
它是一个使用类似线程模块的API来支持产生进程的包。

- 新创建的进程与进程的切换都是要耗资源的，所以平时工作中进程数不能开太大
- 同时可以运行的进程数一般受制于CPU的核数
- 除了使用Process方法，我们还可以使用Pool类创建多进程

```python
'''Learn multiprocess

    Learn multiprocess of python.
'''

import time
import os
from multiprocessing import Process


"""
# singe process
def long_time_task():
    print('当前进程: {}'.format(os.getpid()))
    time.sleep(2)
    print('结果: {}'.format(8 ** 20))

if __name__ == "__main__":
    print('当前母进程: {}'.format(os.getpid()))
    start = time.time()
    for i in range(2):
        long_time_task()
    end = time.time()
    print('用时: {}s'.format((end - start)))
"""

# multiprocess
def long_time_task(i):
    print("子进程: {} - 任务{}".format(os.getpid(), i))
    time.sleep(2)
    print("结果: {}".format(8 ** 20))

if __name__ == '__main__':
    print('当前母进程: {}'.format(os.getpid()))
    start = time.time()
    p1 = Process(target=long_time_task, args=(1,))
    p2 = Process(target=long_time_task, args=(2,))
    print('等待所有子进程完成!')
    p1.start()
    p2.start()
    p1.join()
    p2.join()
    end = time.time()
```

<br>

**多进程间的数据共享与通信**
通常，进程之间是相互独立的，每个进程都有独立的内存。通过共享内存(nmap模块)，进程之间可以共享对象，使多个进程可以访问同一个变量(地址相同，变量名可能不同)。多进程共享资源必然会导致进程间相互竞争，所以应该尽最大可能防止使用共享状态。还有一种方式就是使用队列queue来实现不同进程间的通信或数据共享，这一点和多线程编程类似。

```python
from multiprocessing import Process, Queue
import os, time, random


# share data with multiprocess
# 2 process, one for write, one for read. Implemented sharing a queue
def write(q):
    print('进程写: {}'.format(os.getpid()))
    for value in ['A', 'B', 'C']:
        print('将 {} 放入队列...'.format(value))
        q.put(value)
        time.sleep(random.random())

def read(q):
    print('进程读: {}'.format(os.getpid()))
    while True:
        value = q.get(True)
        print('从队列获取 {}'.format(value))

if __name__ == '__main__':
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    pw.start()
    pr.start()
    # 等待pw结束
    pw.join()
    # pr进程里是死循环，无法等待其结束，只能强行终止
    pr.terminate()
```




<br>
<br/>


### concurrent

concurrent包中只有一个模块
concurrent.futures，启动并行任务
该模块为异步(asynchronously)执行可调用提供了一个高级的接口。


<br>
<br/>


### subprocess

subprocess，子进程管理
该模块允许你生成新的进程，连接到它们的input/output/error pipes，并获得它们返回的代码。
每个进程可以有多个线程。

```py
import subprocess

#在Python脚本中启动一个外部程序
subprocess.Popen(‘/tmp/hello.py’)
#hello world!

#用Popen传递参数，这需要传递一个列表
subprocess.Popen([‘/tmp/hello.py’, 'argv1'])

#它还有许多参数
help(subprocess.Popen)
```


<br>
<br/>
****

### sched

sched，事件调度程序(scheduler)
该模块定义了一个实现通用时间调度器的类。


<br>
<br/>


### queue

queue，一个同步队列类
该模块实现了多生产者、多消费者队列。当信息必须在多线程之间安全地交换时，它在线程编程中特别有用。


<br>
<br/>


## 进程间的通信和网络


本章介绍的模块，提供了不同进程进行通信的机制。


<br>
<br/>


### socket

socket，低级网络接口
该模块提供了对BSD socket的访问。

Socket是网络编程的一个抽象概念。通常我们用一个Socket表示“打开了一个网络链接”，而打开一个Socket需要知道目标计算机的IP地址和端口号，再指定协议类型即可。
tcp需要建立连接，udp不需要建立连接，因此udp每次需要指定发送地址。

socket类型：

- `socket.AF_UNIX`
本机通信

- `socket.AF_INET`
服务器间的网络通信

- `socket.AF_INET6`
IPv6的服务器间的通信

- `socket.SOCK_STREAM`
基于TCP的流式socket通信

- `socket.SOCK_DGRAM`
基于UDP数据包的socket通信

- `socket.SOCK_RAM`
原始套接字

- `socket.SOCK_SEQPACKET`
可靠的连续数据包服务

<br>

```
#服务端socket函数：
bind()    在AF_INET下，以tuple(host, port)的方式传入，如s.bind((host, port))
listen()    可设置挂起的最大连接数
accept()    接收tcp连接并返回(conn, address), conn是新的套接字对象, address是客户端地址


#客户端socket函数：
connect()
connect_ex()


#公共socket函数
#tcp
recv()    接受TCP套接字的数据，数据以字符串形式返回，buffsize指定要接受的最大数据量
send()
sendall()    完整发送tcp数据
#udp
recvfrom()
sendto()

close()

```

<br>

socket编程思想：

```
#Server-side
1. 创建socket
2. 监听
3. 接收client请求
4. 接收C端数据
5. 关闭头街子


#Client-side
1. 创建socket
2. 连接到S端
3. 发送数据
4. 关闭套接字
```

<br>

**注意**
在Python3.x中，byte strings 和 unicodestrings是两种不同的类型，相互之间需要进行`decode()`和`encode()`
`send()`和`recv()`都是bytes类型，需要与str类型进行转换。

<br>

```py
#tcp

#S端
import socket

host = 'localhost'
port = 5678
bf = 1024
maxConn = 3

tcpS = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
tcpS.bind((host, port))
tcpS.listen(maxConn)

print('Server start at {host}:{port}'.format(host=host, port=port))
print('Waiting for connection...')

while True:
    conn, addr = tcpS.accept()
    print('Connected by: {addr}'.format(addr=addr))

    while True:
        data = conn.recv(bf)
        print(data.decode('utf-8'))
        conn.send('server received message.'.encode('utf-8'))
    tcpS.close()



#C端
import socket

host = 'localhost'
port = 5678
bf = 1024

tcpC = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
tcpC.connect((host, port))

while True:
    msg = input('Please input message: \n')
    tcpC.send(msg.encode('utf-8'))
    data = tcpC.recv(bf)
    print(data.decode('utf-8'))

```

```py
#udp

#S端
from pymongo import MongoClient
import socket, datetime


host = 'localhost'
port = 5679
bf = 1024

mongoPort = 27017
mongoUser = 'zhang'
mongoPw = 'password'
mongoDb = 'zhang'
mongoColl = 'udpS'

udpS = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
udpS.bind((host, port))

collection = MongoClient(host=host, port=mongoPort, \
             username=mongoUser, password=mongoPw).zhang.udpS

print('udp socket on {host}:{port}...'.format(host=host, port=port))


while True:
    data, addr = udpS.recvfrom(bf)
    print('Received from {addr}'.format(addr=addr))
    print(data.decode('utf-8'))
    print('\n')

    msg = 'Server has recived!\n'
    udpS.sendto(msg.encode('utf-8'), addr)

    dateTime = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    post = {
      'author': 'Server',
      'date': dateTime,
      'message': data.decode('utf-8')
    }
    collection.insert_one(post)



#C端
from pymongo import MongoClient
import socket, datetime


host = 'localhost'
port = 5679
bf = 1024

mongoPort = 27017
mongoUser = 'zhang'
mongoPw = 'password'
mongoDb = 'zhang'
mongoColl = 'udpC'

udpC = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
collection = MongoClient(host=host, port=mongoPort, \
             username=mongoUser, password=mongoPw).zhang.udpC


while True:
    msg = str(input('Please input message: \n'))
    udpC.sendto(msg.encode('utf-8'), (host, port))
    data = udpC.recv(bf)
    print(data.decode('utf-8'))

    dateTime = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    post = {
      'author': 'Client',
      'date': dateTime,
      'message': data.decode('utf-8')

    collection.insert_one(post)

    udpC.close()
```

<br>
<br/>


### ssl

ssl，套接字对象的TLS/SSL封装
此模块提供了对网络套接字的传输层安全(通常称为安全套接字层)的加密和对等身份验证功能。


<br>
<br/>


### select

select，等待I/O完成
该模块提供对大多数操作系统中可用的`select()`和`poll()`函数的访问。


<br>
<br/>


### selector

selector，高级I/O复用
该模块基于`select()`模块构建，有序高级和高效的I/O复用。


<br>
<br/>


### asyncio

asyncio，异步I/O，事件循环，协同程序和任务
该模块提供了使用协同程序编写单线程并发代码的基础结构，在套接字和其它资源上多路复用I/O访问，运行网络客户端和服务器以及其它相关基元。


<br>
<br/>


### asyncore

asyncore，异步套接字处理器
该模块为编写异步套接字服务(客户端和服务端)提供了基本的基础结构。


<br>
<br/>


### asynchat

asynchat，异步套接字命令/响应处理器
该模块构建在asyncore之上，简化了异步客户端和服务端，并更容易处理其元素被任何字符串终止和长度可变的协议。


<br>
<br/>


### signal

signal，为异步事件设置处理器
该模块提供了在Python中使用信号处理程序的机制。


<br>
<br/>


### mmap

mmap，内存映射文件支持
内存映射文件对象的行为与bytearray和文件对象类似。


<br>
<br/>


## 网络数据处理


本章介绍的模块，支持处理常用网络数据格式。


<br>
<br/>


### email

email，一个email和MIME处理包
该包是用于管理电子邮件信息的库，包含MIME和其它基于RFC 2822的消息文档。

MIME(Multipurpose Internet Mail Extensions)多用途互联网邮件扩展，是一种标准化的方式来标识文档的性质和格式。浏览器通常使用MIME类型(而不是文件扩展名)来确定如何处理文档。

栗子：

```
type/subtype


text/plain
text/html
image/jpeg
image/png
audio/mpeg
audio/ogg
audio/*
video/mp4
application/octet-stream
…
```

<br>

#### email.message

表示一个电子邮件信息

<br>

#### email.parser

解析电子邮件信息

<br>

#### email.generator

生成MIME文档

<br>

#### email.policy

政策对象

<br>


#### email.headerregistry

自定义头对象

<br>

#### email.contentmanager

管理MIME内容

<br>

#### email.mime

从抓挠中创建电子邮件和MIME对象。

<br>

#### email.header

Internationalized headers

<br>

#### email.charset

表示字符集

<br>

#### email.encoders

编码器

<br>

#### email.errors

异常和缺陷类

<br>

#### email.utils

各种各样的功能

<br>

#### email.iterators

迭代器


<br>
<br/>



### json


json，JSON编码器和解码器
JSON(JavaScript Object Notation)，是一个受JavaScript对象语法启发的轻量级的数据交换格式。

json只能包含如下Python数据类型的值：

- 字符串
- 整型
- 浮点数
- 布尔型
- 列表
- 字典
- NoneType

<br>

```
import json

JSONDATA = '{"name": "zhang", "age": 21, "likeFootball": true}

loadData = json.loads(JSONDATA)
dumpData = json.dumps(jsonData)

```


<br>
<br/>


### mailcap

mailcap，mailcap文件处理
mailcap文件用于配置感知MIME的应用程序(如邮件阅读器和Web浏览器)，如何对具有不同MIME类型的文件做出反应。


<br>
<br/>


### mailbox

mailbox，以各种格式操作邮箱
该模块定义了两个类: Mailbox和Message，用于访问和操作磁盘邮箱及其包含的邮件。


<br>
<br/>


### mimetypes

mimetypes，将文件名映射到MIME类型
该模块在文件名或URL和MIME类型之间进行转换。


<br>
<br/>


### base64

base64，base16、base32，base64，base85数据编码
该模块提供了将二进制数据编码为可打印的ASCII字符，并将这些编码解码回二进制数据的函数。


<br>
<br/>


### binhex

binhex，编码和解码binhex4文件


<br>
<br/>


### binascii

binascii，在二进制和ASCII之间进行转换
该模块包含了许多方法，用于转换在二进制和各种ASCII编码的二进制表示之进行转换的方法。


<br>
<br/>


### quopri

quopri，编码和解码MIME引用打印数据


<br>
<br/>


### uu

uu，编码和解码uuencode文件
该模块以uuencode格式对文件进行编码和解码，允许任意二进制数据仅通过ASCII连接进行传输。



<br>
<br/>



## 结构化标记处理工具


Python支持用以处理各种形式的结构化数据标记的模块。

- 标准通用标记语言，SGML()
- 超文本标记语言，HTML
- 扩展标记语言，XML


<br>
<br/>


### html

html，支持超文本标记语言
该模块定义了用以操作HTML的实用程序。


<br>
<br/>


### html.parser

html.parser，简单HTML和XHTML解析器
该模块提供了一个类，用来解析HTML和XHTML格式的文本文件的基础。


<br>
<br/>


### html.entities

html.entities，HTML一般实体的定义


<br>
<br/>


### XML处理模块

用于处理XML的Python接口被分组到xml包


<br>
<br/>



## 网络协议


本章介绍的模块，实现了网络协议并支持相关技术。


<br>
<br/>


### webbrowser

webbrowser - Interfaces for launching and remotely controlling Web browsers.
webbrowser，便利的web浏览器控制器
该模块提供了一个高级interface，允许向用户显示基于web的文档。

```
import webbrowser

webbrowser.open('https://www.baidu.com')
```

<br>
<br/>


### cgi

cgi，通用网关接口支持
CGI 脚本的支持模块；
该模块定义了许多用Python编写的CGI脚本的实用功能。


<br>
<br/>


### cgitb

cgitb，CGI脚本的追溯管理器
此模块为Python脚本提供了一个特殊的异常处理程序。


<br>
<br/>


### wsgiref

wsgiref，WSGI功能和参考实现
Web服务器网关接口(WSGI)，是Web服务器软件和Web应用程序(Python编写)之间的标准接口。拥有标准接口可以轻松使用支持WSGI和多个不同Web服务器的应用程序。


<br>
<br/>


### urllb

`urllib`模块，处理URL


<br>
<br/>


### urllib.request

`urllib.request`模块，用于打开URL的可扩展库
该模块定义了函数和类，用于在复杂的世界中打开URL——基本和身份认证，重定向，cookie等


<br>
<br/>


### urllib.response

urllib.response，响应类
该模块定义了向接口这样的最小文件的函数和类。


<br>
<br/>


### urllib.parse

urllib.parse，将URL解析为组件
此模块定义了一个标准接口，用于在组件中分解统一资源定位符(URL)字符串，将组件重新组合为URL，并将相对URL转换为基本URL的绝对URL。


<br>
<br/>


### urlllib.error

urllib.error，由urllib.request引起的异常类
该模块定义了由urllib.request引发的异常类。


<br>
<br/>


### urllib.robotparser

urllib.robotparser，解析robot.txt
此模块提供了一个`RobotFileParser`类，它回答了有关特定用户代理是否可以在发布robots.txt文件的Web站点上获取URL的问题。


<br>
<br/>


### http

http，HTTP模块


<br>
<br/>


### http.client

http.client，HTTP协议客户端
该模块定义了实现HTTP和HTTPS协议客户端的类。


<br>
<br/>


### ftplib

ftplib，FTP协议客户端
此模块定义了FTP类和一些相关项。FTP类实现了FTP协议的客户端。


<br>
<br/>


### poplib

poplib，POP3协议客户端
此模块定义了POP3类，它封装了一个到POP3服务器的连接，并实现了该协议。


<br>
<br/>


### imaplib

imaplib，IMAP4协议客户端
此模块定义了三个类，封装一个到IMAP服务器的连接，并实现IAP4客户端协议的大部分子集。


<br>
<br/>


### nntplib

nntplib，NNTP协议客户端
此模块定义了NNTP类，它实现网络新闻传输协议(NNTP)客户端。


<br>
<br>


### smtplib

`smtplib`模块，SMTP协议客户端
此模块定义了一个SMTP客户端会话对象，可使用SMTP守护进程发送邮件给任一互联网计算机。

```py
import smtplib

#send = smtplib.STMP('smtp.example.com', port=xxx)
send = smtplib.SMTP_SSL(‘smpt.exmail.qq.com’, 465)

send.helo()
#(250, b'smtp.qq.com')

#登录需要提前设置邮箱授权码，使用授权码作为密码登录
send.login(user, passed)

send.sendmail(from, to, message)
send.quti()
```

### smtpd

smtpd，SMTP服务器
该模块提供了几个类来实现SMTP服务器。


<br>
<br/>


### telnetlib

telnetlib，Telnet客户端
此模块提供了一个telnet类，用于执行Telnet协议。


<br>
<br/>


### uuid

uuid，UUID对象
此模块提供了不可修改的UUID对象和uuid[1-5]函数。


<br>
<br/>


### socketserver

socketserver，一个网络服务器的框架
此模块简化了编写网络服务器的任务。


<br>
<br/>


### http.server

http.server，HTTP服务器
此模块定义了类，用于实现HTTP服务器。


<br>
<br/>


### http.cookie

http.cookie，HTTP状态管理
此模块定义了类，用于抽象cookie概念(HTTP状态管理机制)。


<br>
<br/>


### http.cookiejar

http.cookiejar，HTTP客户端的cookie处理
此模块定义了类，用于自动处理HTTPcookie。


<br>
<br/>


### xmlrpc

xmlrpc，XMLRPC服务器和客户端模块
XML-RPC是一种远程过程调用方法，它使用通过HTTP传递的XML传输。


<br>
<br/>


### xmlrpc.client

xmlrpc.client，XML-RPC客户端访问


<br>
<br/>


### xmlrpc.server

xmlrpc.server，基本的XML-RPC服务器


<br>
<br/>


### ipaddress

ipaddress，IPv4/IPv6操作库
此模块提供了创建、修改和操作IPv4和IPv6和网络的功能。


<br>
<br/>
<br/>


## 多媒体服务


本章介绍的模块，实现了用于多媒体应用的各种算法和接口。


<br>
<br/>


### audioop

audioop，操作原始音频数据
此模块包含一些对声音片段有用的操作。


<br>
<br/>


### aifc

aifc，读写AIFF和AIFC文件
此模块提供了对读写AIFF和AIFC文件的支持。

-  AIFF is Audio Interchange File Format
	+ 一种用于将数字音频样本存储在文件中的格式
-  AIFC是一种更新的格式，包括压缩音频数据


<br>
<br/>


### sunau

sunau，读写Sun AU文件
此模块为Sun AU声音格式提供了一个便利的接口。


<br>
<br/>


### wave

wave，读写WAV文件
此模块为WAV声音格式提供了一个便利的接口。


<br>
<br/>


### chunk

chunk，读取IFF分块数据
此模块为读取使用EA IFF块的文件提供了接口。


<br>
<br/>


### colorsys

colorsys，颜色系统之家的转换
此模块定义了计算机显示器RGB和其它三个坐标系统：YIQ, HLS, HSV中使用的RGB颜色空间中表示的颜色之间的颜色值的双向转换。


<br>
<br/>


### imghdr

imghdr，确定图像类型
此模块确定文件或字节流中包含的图像类型。


<br>
<br/>


### sndhdr

sndhdr，确定声音文件类型
此模块提供了实用功能，视图确定文件中的声音数据类型。


<br>
<br/>


### ossaudiodev

ossaudiodev，访问与OSS兼容的音频设备
此模块允许你访问OSS(open sound system)音频接口。OSS是Linux和FreeBSD的标准音频接口。


<br>
<br/>
<br/>



## 语言环境


本章介绍的模块，可帮助你编写独立于语言和语言环境的软件。


<br>
<br/>


### gettext

gettext，多语言国际化服务
此模块为你的Python模块和应用程序提供了国际化和本地化服务。


<br>
<br/>


### locale

`locale`语言环境模块，打开对POSIX语言环境数据库和功能的访问。


<br>
<br/>
<br/>



## 程序框架


本章介绍的模块，是基本上决定程序结构的框架。


<br>
<br/>


### turtle

turtle，乌龟图形
乌龟图形是向孩子们介绍编程的一种流行方式。


<br>
<br/>


### cmd

cmd，支持面向行的命令解释器
此类为编写面向行的命令解释器提供了一个简单的框架。


<br>
<br/>


### shlex

shlex，简单的词法分析
此类可以容易地编写词法分析器，以获得类似Unix shell的简单语法。



<br>
<br/>
<br/>


## 带有Tk的图形用户界面


Tk/Tcl是Python的一部分。它提供了一个强大且独立于平台的窗口工具包，可供Python程序员使用的tkinter包。

- Tcl(Tool Command Language)，是一种脚本语言
- Tk，是基于Tcl的图形界面开发工具箱


<br>
<br/>


### tkinter

tkinter，与Tcl/Tk的Python接口
此包是到Tk GUI工具箱的标准Python接口。


<br>
<br/>


### tkinter.ttk

tkinter.ttk，Tk主题小部件
此模块提供了对Tk主题小部件集的访问。


<br>
<br/>


### tkinter.tix

tkinter，Tk扩展小工具
此模块提供了一组额外的小工具。


<br>
<br/>


### tkinter.scrolledtext

滚动(scrolled)文本工具
此模块提供了一个相同名称的类，它实现了基本的文本小部件，具有一个垂直滚动条，用于执行正确的事情。


<br>
<br/>


### IDEL

IDEL是Python的集成开发和学习环境。


<br>
<br/>


### 其它GUI包

- PyGObject
- PyGTK
- PyQt
- PySide
- wxPython



<br>
<br/>
<br/>



## 开发工具


本章介绍的模块可帮助你你编写软件。

开发高质量软件的一种方法是在开发过程中为每个函数编写测试，并在开发过程中频繁运行这些测试。

<br>
<br/>


### typing

typing，支持类型提示
此模块支持PEP 484指定的类型提示。


<br>
<br/>


### pydoc

pydoc，文档生成器和在线帮助系统
此模块从Python模块自动生成文档，文档可作为控制台上的文本页面呈现，提供个Web浏览器或保存到HTML文件。


<br>
<br/>


### doctest

`doctest`模块，测试交互式Python示例
此模块搜索类似于交互式Python会话的文本片段，然后执行这些会话以验证它们是否完全安装所示工作。




<br>
<br/>


### unittest

unittest，单元测试框架


<br>
<br/>


### 2to3

2to3，自动翻译Python2-3代码
获取Python2源代码并应用一系列修复程序将其转换为有效的Python3代码。


<br>
<br/>


### test

test，用于Python的回归测试包
此包包含了Python的所有回归测试。


<br>
<br/>


### test.support

test.support，Python测试套件功能


<br>
<br/>
<br/>



## 调试和分析

- 调试器(Debugger)使你能遍历代码，分析堆栈并设置断点
- 分析器(Profiler)运行代码并给出执行时间的详细分类，使你识别程序中的瓶颈


<br>
<br/>


### bdb

bdb，调试器框架
此模块处理基本的调试器功能。


<br>
<br/>


### faulthandler

faulthandler，转储Python回溯(traceback)


<br>
<br/>


### pdb

pdb，Python调试器
此模块为Python程序定义了一个交互式源代码调试器。


<br>
<br/>


### Python分析器

cProfile和profile提供了Python程序的确定性分析。


<br>
<br/>


### timeit

`timeit`模块，测量小代码片段的执行时间
此模块提供了一个简单的方法类计算一小段Python代码的时间。

```py
>>> from timeit import Timer
>>> Timer('a,b = b,a', 'a=1; b=2').timeit()
0.020318730967119336
```


<br>
<br/>


### trace

trace，追踪Python语句的执行
此模块允许你追踪程序执行，生成带注释的语句覆盖列表，打印调用关系和在程序运行期间执行的函数列表。


<br>
<br/>


### tracemalloc

tracemalloc，追踪内存分配
此模块是一个追踪由Python分配的内存块的调试工具。


<br>
<br/>
<br/>



## 软件打包和分发


这些库可帮助你发布和安装Python软件。这些模块被设计来与PyPi结合使用，但它们也可以与本地索引服务器一起使用，或根本不需要任何索引服务器。


<br>
<br/>


### distutils

distutils，构建和安装Python模块
此软件包为构建和安装其它模块到Python提供支持。


<br>
<br/>


### ensurepip

ensurepip，引导pip安装程序
此软件包支持将pip安装程序引导到现有的Python或虚拟环境中。


<br>
<br/>


### venv

venv，创建虚拟环境
此模块为创建轻量虚拟环境提供支持，可选地域系统目录隔离。


<br>
<br/>


### zipapp

zipapp，管理可执行的python zip归档
Python提供了管理创建包含Python代码的zip文件的工具。


<br>
<br/>
<br/>



## Python服务组件


本章介绍的模块，提供了与Python解释器及其与环境交互相关的各种服务。


<br>
<br/>


### sys

`sys`模块，系统特定的参数和功能
此模块提供了对解释器使用或维护的一些变量以及与解释器交互的函数非访问。

**命令行参数**

```py
import sys
print(sys.argv)
```

**错误输出重定向和程序终止(termination)**
`sys`模块还具有stdin, stdout, stderr属性。

```py
>>> sys.stderr.write('Warning, log file not found starting a new one\n')
Warning, log file not found starting a new one
```


<br>
<br/>


### sysconfig

sysconfig，提供对Python配置信息的访问
此模块提供对Python配置信息的访问，如安装路径列表和当前平台相关的配置变量。


<br>
<br/>


### builtins

builtins，内建对象
此模块提供了对Python所有内置标识符的直接访问。例如，`builtins.open`是内建函数`open()`的全名。


<br>
<br/>


### `__main__`

`__main__`，顶级脚本环境
`__main__`是顶级代码执行的范围的名称。从标准输入、脚本或交互式提示读取时，模块的`__name__`设置为等于`__main__`


<br>
<br/>


### warnings

warnings，警告控制
警告信息通常在有用的情况下发出，以提醒用户程序中的某些条件，该条件不能保证引发异常并终止程序。
Python程序员通过调用此模块中的`warn()`函数来发出警告。


<br>
<br/>


### contextlib

contextlib，with语句上下文实用程序
此模块为涉及with语句的常见任务提供使用程序。


<br>
<br/>


### abc

abc，抽象基类(Abstract Base Classes)
此模块提供了在Python中定义抽象基类的基础结构。


<br>
<br/>


### atexit

atexit，退出处理程序
此模块定义了注册和注销清理函数的函数。


<br>
<br/>


### traceback

traceback，打印或取回堆栈回溯
该模块提供了一个标准接口，用来提取、格式化和打印Python程序的堆栈追踪。


<br>
<br/>


### `__future__`

`__future__`，未来的声明定义


<br>
<br/>


### gc


gc，垃圾收集器接口(Garbage Collector interface)
此模块为可选的垃圾收集器提供了一个接口。


<br>
<br/>


### inspect

inspect，检查活对象(Inspect live objects)
此模块提供了几个有用的功能来帮助获取有关活动对象的信息，如模块、类、函数、回溯、框架对象和代码对象。


<br>
<br/>


### site

site，Site-specific configuration hook


<br>
<br/>


### fpectl

fpectl，浮点异常控制(Floating point exception control)



<br>
<br/>
<br/>



## 自定义Python解释器


本章介绍的模块，允许编写类似于Python的交互式解释器接口。


<br>
<br/>


### code

code，解释器基本类
此模块提供了一些工具，来实现Python的read-eval-print循环。


<br>
<br/>


### codeop

codeop，编译Python代码
此模块提供了实用程序，用于模拟Python read-eval-print循环，像code模块中做的那样


<br>
<br/>
<br/>



## 导入模块


本章介绍的模块，提供了导入其它Python模块和以自定义导入进程的hook的新方法。


<br>
<br/>


### zipimport

zipimport，从zip归档文件导入模块
此模块增加了从Zip格式的归档中导入Python模块和软件包的功能。
通常不需要明确使用zipimport模块，内置导入机制将自动使用zip归档文件的路径(sys.path)。


<br>
<br/>


### pkgutil

pkgutil，包扩展程序
此模块为导入system提供实用程序，尤其是软件包的支持。


<br>
<br/>


### modulefinder

modulefinder，查找脚本使用的模块
此模块可用于确定脚本导入的模块集。


<br>
<br/>


### runpy

runpy，定位和执行Python模块
此模块用于定位和运行Python模块，而不必先导入它们。


<br>
<br/>


### importlib

importlib，执行import
此软件包有两个目的：

- 在Python源代码中提供import语句的实现(`__import__`函数)
- 实现import组件暴露在此软件包中，使用户更容易创建它们自己的定制对象参与导入过程


<br>
<br/>
<br/>



## Python语言服务


Python提供了许多模块来协助处理Python语言。包括：

- 标记
- 解析
- 语法分析
- 字节码反汇编
- ...


<br>
<br/>


### parser

parser，访问Python解析树
此模块为python内部解析器和字节码编译器提供了一个接口。


<br>
<br/>

### ast

ast，抽象语法树(Abstract Syntax Trees)
此模块帮助Python应用程序处理Python抽象语法的树。


<br>
<br/>


### symtable

symtable，访问编译器的符号
符号表由AST编译器在字节码生成之前生成。


<br>
<br/>


### symbol

symbol，用于Python解析的常量
该模块提供了，表示解析树内部节点数值的常量。


<br>
<br/>


### token

token，与Python解析树一起使用的常量
此模块提供了，表示解析树(终端令牌)的叶子节点数值的常量。


<br>
<br/>


### keyword

keyword，测试Python关键字
此模块允许Python程序确定字符串是否为关键字。


<br>
<br/>


### tokenize

tokenize，用于Python源代码的令牌器
此模块为Python源代码提供了一个用Python实现的语言扫描器。


<br>
<br/>


### tabnanny

tabnanny，检查不明确的缩进(Detection of ambiguous indentation)


<br>
<br/>


### pyclbr

pyclbr，Python类浏览器支持
此模块可用于，确定有关模块中定义的类、方法和顶级函数的一些限制信息。


<br>
<br/>


### py_compile

`py_compile`，编译Python源文件
此模块提供了功能，从源文件生成字节码文件，以及当模块源文件作为脚本被调用时使用。


<br>
<br/>


### compileall

compileall，字节编译Python库
此模块提供了实用功能来支持安装Python库。


<br>
<br/>


### dis

dis，用于Python字节码的反汇编器
此模块支持通过反汇编来支持CPython字节码的分析。


<br>
<br/>


### pickletools

pickletools，pickle开发者的工具
此模块包含了各种常量，涉及到pickle模块的细节，一些关于实现的冗长的评论，一些用于分析pickle数据的有用函数。


<br>
<br/>
<br/>



## 杂项服务

本章介绍的模块，提供了在所有Python版本中可用的杂项(miscellaneous)服务。


<br>
<br>


### formatter

formatter，通用输出格式
此模块支持两种接口定义，每种都有多种实现方式：

- 格式化接口
- 格式化接口所需的写入接口


<br>
<br/>
<br/>



## Windows特定服务


本章介绍的模块仅可在MS windows平台上可获取。


<br>
<br/>


### msilib

msillib，读写微软安装程序文件
此模块支持创建Microsoft Installer (`.msi`) 文件。


<br>
<br/>


### msvcrt

msvcrt，MS VC++运行时的有用例程
此函数可访问Windows平台上的一些有用功能。


<br>
<br/>


### winreg

winreg，Windows注册表访问
此模块将Windows注册表的API暴露给Python。


<br>
<br/>


### winsound

winsound，Windows的声音播放接口
此模块提供了对Windows平台提供的基本声音播放机器的访问。


<br>
<br/>
<br/>



## Unix特定服务


本章介绍的模块，提供了Unix操作系统(Unix-Like)特有的功能的接口。


<br>
<br/>


### posix

posix，最基本的POSIX系统调用
此模块提供了对由C标准和POSIX标准 标准化的操作系统功能的访问。


<br>
<br/>


### pwd

pwd， The password database
此模块提供了对Unix用户账户和密码数据库的访问。

```py
import pwd

pwd.getpwdnam('zhang')
pwd.struct_passwd(pw_name='zhang', pw_passwd='x', pw_uid=1000, pw_gid=1000, pw_gecos='zhang', pw_dir='/home/zhang', pw_shell='/bin/bash')
```


<br>
<br>


### spwd

spwd，The shadow password database
此模块提供了对Unix shadow password database的访问。


<br>
<br/>


### grp

grp，The group database
此模块提供了对Unix group database的访问。


<br>
<br/>


### crypt

crypt，Function to check Unix passwords
此模块实现crypt(3)例程的接口，该例程是基于修改的DES算法的单向散列函数。


<br>
<br/>


### termios

termios，POSIX风格的tty控件
此模块提供了一个接口，用于I/O控制的POSIX调用。


<br>
<br/>


### tty

tty，终端控制函数
此模块定义了将tty置入cbreak和raw模式的函数。


<br>
<br/>


### pty

pty，伪(Pseudo)终端程序
此模块定义了处理伪终端概念的操作： 启动另一个进程并以编程方式写入和读取其控制终端。


<br>
<br/>


### fcntl

fcntl，The fcntl and ioctl system calls
此模块对文件描述符执行文件控制和I/O控制。


<br>
<br/>


### pipes

pipes，shell pipelines的接口
此模块定义了一个类来抽象管道的概念——从一个文件到另一个文件的一系列转换器。


<br>
<br/>


### resource

resource，资源使用信息
此模块提供了测量和控制程序使用系统资源的基本机制。


<br>
<br/>


### syslog

syslog，Unix syslog library routines
此模块为Unix系统日志库例程提供了一个接口。


<br>
<br/>

---

<br/>



# 第三方库


基本上可将第三方库理解为开源库！

<br>

Awesome-Python: <https://github.com/jobbole/awesome-python-cn>
PyPI: <https://pypi.org/>


<br>
<br/>


## 系统管理

- sh
- Watchdog


<br/>
<br/>


## 数据库

- PyMySQL
- pymongo
- redis


<br/>


### PyMySQL

PyMySQL：<https://pypi.org/project/PyMySQL/>

<br>

**首先创建数据库**

```sql
CREATE TABLE `users` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `email` varchar(255) COLLATE utf8_bin NOT NULL,
    `password` varchar(255) COLLATE utf8_bin NOT NULL,
    PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin
AUTO_INCREMENT=1 ;
```

<br>

**连接**

```py
import pymysql

connection = pymysql.connect(
    host='localhost',
    user='username',
    password='password',
    port=3306,
    db='DBname',
    charset='utf8',
    cursorclass=pymysql.cursors.DictCursor)

try:
    with connection.cursor() as cursor:
        sql = "INSERT INTO `users` (`email`, `password`) VALUES (%s, %s)"
        cursor.execute(sql, ('webmaster@python.org', 'very-secret'))

    #commit to save
    connection.commit()

    with connection.cursor() as cursor:
        sql = "SELECT `id`, `password` FROM `users` WHERE `email`=%s"
        cursor.execute(sql, ('webmaster@python.org',))
        result = cursor.fetchone()
        print(result)
finally:
    connection.close()
```


<br/>
<br/>


### pyMongo

pyMongo Docs: <https://api.mongodb.com/python/current/>

pyMongo是一个用于使用MongoDB的工具的Python发行版，并且是从Python工作于MongoDB的推荐方式。

<br>

**依赖**

- mongodb
- pyMongo

<br>

**连接**

```py
from pymongo import MongoClient

#host and port
client = MongoClient('localhost', 27017)

#url format
client = MongoClient('mongodb://localhost:27017')


#认证
client = MongoClient(host='localhost', port=27017, username='user', password='pass')
```

<br>

**获取数据库**

```py
db = client.${database}

#or

db = client['${database}']
```

<br>

**获取集合**

```py
collection = db.${collection}

#or
collection = db['${collection}']
```

<br>

**文档**

```py
import datetime

post = {
  '_id': 'post01',
  'author': 'Zhang21',
  'text': 'My first post!',
  'tags': [ 'mongodb', 'python', 'pymongo' ],
  'date': datetime.datetime.now()
}
```

<br>

**插入文档**

```py
#新建集合
${collection} = db.posts
${collection}.insert_one(post)

#已有集合
collection.insert_one(post)
```

**批量插入**

```py
new_post = [
  {
    '_id': 'post02',
    'author': 'Zhang02',
    'text': '2nd post',
    'tags': ['bulk', 'insert'],
    'date': datetime.datetime.now()
  },
  {
    '_id': 'post03',
    'author': 'Zhang03',
    'text': '3rd post',
    'tags': ['bulk', 'insert'],
    'date': datetime.datetime.now()
  }
]

collection.insert_many(new_post)
```

<br>

**获取文档**

```py
collection.find_one()

collection.find_one({ '_id': 'post01'})
#or
collection.find_one({'author': 'Zhang21'})


import pprint

pprint.pprint(collection.find_one({ '_id': 'post01'}))
```

**查询多个文档**

```py
for post in collection.find():
    pprint.pprint(post)

{'_id': 'post01',
 'author': 'Zhang21',
 'date': datetime.datetime(2018, 6, 14, 11, 13, 11, 372000),
 'tags': ['mongodb', 'python', 'pymongo'],
 'text': 'My first post!'}
{'_id': 'post02',
 'author': 'Zhang02',
 'date': datetime.datetime(2018, 6, 14, 11, 34, 47, 93000),
 'tags': ['bulk', 'insert'],
 'text': '2nd post'}
{'_id': 'post03',
 'author': 'Zhang03',
 'date': datetime.datetime(2018, 6, 14, 11, 34, 47, 93000),
 'tags': ['bulk', 'insert'],
 'text': '3rd post'}


#or
for post in collection.find({'tags': ['bulk', 'insert']}):
     pprint.pprint(post)

{'_id': 'post02',
 'author': 'Zhang02',
 'date': datetime.datetime(2018, 6, 14, 11, 34, 47, 93000),
 'tags': ['bulk', 'insert'],
 'text': '2nd post'}
{'_id': 'post03',
 'author': 'Zhang03',
 'date': datetime.datetime(2018, 6, 14, 11, 34, 47, 93000),
 'tags': ['bulk', 'insert'],
 'text': '3rd post'}
```

<br>

**删除文档**

```py
collection.delete_one({"_id" : "post01"})


#删除多个
collection.delete_many({"_id" : "post02", "_id" : "post03"})
```

<br>

**计数**

```py
collection.count()
3

collection.count({'tags': ['bulk', 'insert']})
2
```

<br>

**索引

```py
result = db.profiles.create_index([('user_id', pymongo.ASCENDING)],
                                 unique=True)
sorted(list(db.profiles.index_information()))
[u'_id_', u'user_id_1']
```


<br/>
<br/>


### redis

The Python interface to the Redis key-value store.
redis模块: <https://pypi.org/project/redis/>

<br>

redis模块提供两个类Redis和StrictRedis用于实现Redis的命令:

- redis.Strictredis(推荐)
StrictRedis用于实现大部分官方的命令，并使用官方的语法和命令

```
help(redis.StrictRedis)

__init__(self, host='localhost', port=6379, db=0, password=None, socket_timeout=None, socket_connect_timeout=None, socket_keepalive=None, socket_keepalive_options=None, connection_pool=None, unix_socket_path=None, encoding='utf-8', encoding_errors='strict', charset=None, errors=None, decode_responses=False, retry_on_timeout=False, ssl=False, ssl_keyfile=None, ssl_certfile=None, ssl_cert_reqs=None, ssl_ca_certs=None, max_connections=None)
```

- redis.Redis(不推荐)
Redis是StrictRedis的子类，用于向后兼容旧版本的redis模块



<br>

**连接**

```py
import redis

r = redis.StrictRedis()
#or
r = redis.StrictRedis(host='localhost', port=6379, db=0, password='password')


#字符串操作
r.set('name', 'Zhang21')
r.get('name')
r.type('name')
r.delete('name')


#列表操作
r.rpush('LIST', 'list-01', 'list-02')
r.type('LIST')
r.llen('LIST')

#help(r.lrane)
#lrange(name, start, end)
lrange('LIST', 0, -1)


#其它redis数据类型操作方法类同
```

<br>


**Connection Pools**
假设Redis服务器与客户端分处在异地，虽然基于内存的Redis数据库有着超高的性能，但是底层的网络通信却占用了一次数据请求的大量时间，因为每次数据交互都需要先建立连接，假设一次数据交互总共用时30ms，超高性能的Redis数据库处理数据所花的时间可能不到1ms，也即是说前期的连接占用了29ms，连接池则可以实现在客户端建立多个链接并且不释放，当需要使用连接的时候通过一定的算法获取已经建立的连接，使用完了以后则还给连接池，这就免去了数据库连接所占用的时间。


```py
#help(redis.ConnectionPool)

pool = redis.ConnectionPool()
#or
pool = redis.ConnectionPool(host='localhost', port=6379, db=0, passeord='password')

r = redis.StrictRedis(connection_pool=pool)

```




<br/>
<br/>


## Web抓取


- request
- BeautifulSoup
- selenium


<br/>
<br/>


### requests

从Internet上下载文件和网页。

```
import requests, pprint

#help(requests)

r = request.get('https://www.baidu.com')
r.status_code
r.headers
r.url
r.text
pprint.pprint(r.text)
```


<br/>
<br/>


### beautifulsoup

解析HTML

```
pip3 install beautifulsoup4

import bs4
```

栗子：
```
import requests, bs4

r = request.get('https://www.baidu.com')
soup = bs4.BeautifulSoup(r.text)
type(soup)

#soup.select()
#soup.find()
```


<br/>
<br/>


### selenium

启动并控制一个Web浏览器。selenium能够填写表单，并模拟鼠标在此浏览器找那个点击

```
from selenium import webdriver

browser = webdriver.Firefox()
browser.get('https://www.baidu.com')
```



<br/>
<br/>


## 文档处理


- openpyxl
- PyPDF2
- pytho-docx


<br/>
<br/>


### openpyxl

openpyxl is a Python library to read/write Excel 2010 xlsx/xlsm/xltx/xltm files.

**关于Excel电子表格：**
一个Excel电子表格文档称为一个工作簿。一个工作簿保存在扩展名为`.xlsx`的文件中。每个工作簿可以包含多个表(工作表)。用户当前查看的表被称为活动表。
每个表有一些列(地址为从A开始的字母)，一些行(地址从1开始的数字)。在特定行和列的方格被称为单元格。单元格形成的网格和数据构成了表。

```
pip3 install openpyxl

import openpyxl

workbook = openpyxl.load_workbook('/tmp/test.xlsx')
type(workbook)
#<class 'openpyxl.workbook.workbook.Workbook'>

workbook.get_sheet_names()
#['Sheet1', 'Sheet2', 'Sheet3']

sheet1 = workbook.get_sheet_by_name('Sheet1')
type(sheet1)

sheet1.title
#'Sheet1'

workbook.get_active_sheet()
#<Worksheet "Sheet1">

sheet1['A1'].value
#'1A'
sheet1['A1'].row
#1
sheet1['A1'].colume
#A

sheet1.cell(row=2, column=2).value
#2B

```


<br/>
<br/>


### PyPDF2

PDF和Word文档是二进制文件，它们比文本文件要复制得多。

```
pip3 install PyPDF2


import PyPDF2

pdfFile = open('/tmp/test.pdf', 'rb')
pdfReader = PyPDF2.pdfFileReader(pdfFile)
pdfWriter = PyPDF2.pdfFileWriter()

page = pdfReader.getPage()
page.extractText()

```


<br/>
<br/>


### python-docx


利用python-docx模块，Python可创建和修改Word文档，它带有`.docx`文件扩展名。

```
pip3 insntall python-docx


import docx

doc = docx.Document('/tmp/test.docx')
len(doc.paragraphs)

#paragraphs和run属性
doc.paragraphs[0].text
doc.paragraphs[0].run[0].text

#写入
doc.add_paragraph('Add line01')
doc.add_paragraph('Add line02').add_run('tail !')
doc.save('/tmp/test.docx')

#标题
doc.add_heading('Header 0', 0)
doc.add_heading('Header 4', 4)

#分页
doc.add_page_broke()

#图像
doc.add_picture(xxx)
```



<br/>
<br/>



## 图像处理


- pillow(PIL)


<br/>
<br/>


### pillow

PIL - the Python Imaging Library.

请了解RGB和CMYK颜色方式。

```
pip3 install pillow

import PIL
```



<br/>
<br/>



## 日志处理

- elasticsearch


<br/>
<br/>


### elasticsearch

Python Elasticsearch Client

pypi: <https://pypi.org/project/elasticsearch>
github: <https://github.com/elastic/elasticsearch-py>
docs: <https://elasticsearch-py.readthedocs.io>

<br>

几个ES概念：

- index
- document
- type
- id

<br>

安装:

```sh
pip3 install elasticsearch
```

栗子：

```py
from datetime import datetime
from elasticsearch import Elasticsearch


#curl localhost:9200/?pretty
#default http://localhost:9200
es=Elasticsearch()
es.info()

#auth
#es=Elasticsearch('https://url:port', http_auth=('elastic', 'passwd'))
#es.info

#ssl
from ssl import create_default_context
es = Elasticsearch('https://url:port', ssl_context=context, http_auth=('ealstic', 'passwd'))

#es.index
#es.create
#es.update
#es.delete

#创建索引
es.indices.create(index='my-index')
#{'acknowledged': True, 'shards_acknowledged': True, 'index': 'my-index'}
#curl localhost:9200/_cat/indices

#添加或修改某个索引的文档格式
es.index(index='my-index', doc_type='test-type', id=2018, body={'any': 'data', 'timestamp': datetime.now()})
#es.create(index='my-index', doc_type='test-type', id=2018, body={'any': 'data', 'timestamp': datetime.now()})
#{'result': 'created', '_primary_term': 1, '_index': 'my-index', '_shards': {'total': 2, 'failed': 0, 'successful': 1}, '_type': 'test-type', '_id': '2018', '_version': 1, '_seq_no': 0}

#查看索引
es.get(index='my-index', doc_type='test-type', id=2018)
#{'_index': 'my-index', '_source': {'timestamp': '2018-07-18T11:34:49.573721', 'any': 'data'}, '_type': 'test-type', 'found': True, '_id': '2018', '_version': 1}

#不指定id，es会自动生成，但查询时候需要id
data={
  'timestamp': datetime.now(),
  'name': 'zhang21',
  'msg': 'Hello'
}
es.index(index='my-index', doc_type='test-type', body=data)
#{'result': 'created', '_primary_term': 1, '_index': 'my-index', '_shards': {'total': 2, 'failed': 0, 'successful': 1}, '_type': 'test-type', '_id': 'C_vnq2QBmuTERb-Wz39W', '_version': 1, '_seq_no': 0}
es.get(index='my-index', doc_type='test-type', id='C_vnq2QBmuTERb-Wz39W')
#{'_index': 'my-index', '_source': {'name': 'Zhang21', 'timestamp': '2018-07-18T13:40:04.005192', 'msg': 'Hello'}, '_type': 'test-type', 'found': True, '_id': 'C_vnq2QBmuTERb-Wz39W', '_version': 1}

#查询
es.search(index='my-index')


#批量操作
from elasticsearch import helper
help(helper.bulk)
#bulk()支持index, create, delete, upsate动作
package=[]
for i in range(5):
    rom={
	  'count': i,
	  'timestamp': datetime.now()
	}
	package.append(row)

actions=[
  {
    '_op_type': 'index',
	'_index': 'my-index',
	'_type': 'test-type',
	'_source': i
  }
    for i in package
]
pprint(actions)
helpers.bulk(es, actions)
pprint(es.search(index='my-index'))
```

具体信息请查看文档！



<br/>
<br/>
<br/>



## 数据分析

基于《Python Data Analysis》一书！
强烈建议使用[Anaconda](https://www.anaconda.com/)安装Python和Jupyter。

- ipython
- jupyter
- pandas
- numpy
- statsmodels
- matplotlib


<br/>
<br/>


### Anaconda

- site: <anaconda.com>
- doc: <docs.anaconda.com/anaconda/>
- 参考: <www.zhihu.com/question/58033789/answer/254673663>

Anaconda 是一种Python语言的免费增值开源发行版，用于进行大规模数据处理, 预测分析, 和科学计算, 致力于简化包的管理和部署。Anaconda使用软件包管理系统Conda进行包管理。
你可能已经安装了Python，那为什么还需要Anaconda？

- Anaconda附带了一大批常用的数据科学包
- Conda管理包
- 管理环境

<br>

**安装**
到官网下载不同平台的包进行安装。

```
wget https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh

bash ./Anaconda3-5.2.0-Linux-x86_64.sh
#之后可设置安装路径和环境变量


#查看
conda --version

#更新所有包
conda upgrade --all
```

<br>

**包管理**
conda is a tool for managing and deploying applications, environments and packages.

```sh
#它会自动安装依赖
#其实和pip差不多
conda install <package>
conda install requests=1.10.0
conda install pandas numpy

#卸载
conda remove <package>


#更新
conda update <package>
conda update <package> --all


#列出
conda list


#搜索
conda search
```

<br>

**环境管理**
为不同项目创建不同的运行环境。

```
#conda create -h
#创建环境
#默认为 ~/.conda/envs/<evn_name>
conda create -n <env_name> <package_names>
conda create -n py3 pandas


#指定Python版本
conda create -n py3 python=3
conda create -n py2 python=2
conda create -n py36 python=3.6


#使用环境
source activate <env_name>
#或
conda activeate <env_name>

#关闭环境
source deactivate
#或
conda deactivate


#自定义目录
conda create -p /path/py2 python=2.7


#删除环境
conda env remove -n <env_name>


#列出环境
conda env list


#查看环境库
conda list -n <env_name>


#环境变量
#导出
cond env export > envName.yaml
#或
pip freeze > evnName.txt


#导入
conda env update -f=/path/envName.yaml
#或
pip install -r /path/envName.txt


#列出
conda env list

```


<br/>
<br/>


### ipython

- site: <ipython.org>
- github: <github.com/ipython>
- pypi: <pypi.org/project/ipython/>

<br>

Python Shell有很多弊端，所以使用功能更强大的ipython。
ipython提供了丰富的工具包，可帮助你以交互的方式充分利用Python:

- 强大的交互式Shell
- Jupyter的内核
- 支持交互式数据可视化和GUI工具箱
- 灵活，可嵌入式的解释器，可加载到自己的项目中
- 使用方便，高性能的并行计算工具

<br>

安装:

```sh
#bash
sudo pip3 install ipython


#使用Anaconda
conda install ipython
#启动
ipython
Python 3.6.6 |Anaconda, Inc.| (default, Jun 28 2018, 17:14:51)
Type 'copyright', 'credits' or 'license' for more information
IPython 6.5.0 -- An enhanced Interactive Python. Type '?' for help

```




<br/>
<br/>


### jupyter

- site: <jupyter.org>
- github: <pypi.org/project/jupyter/>
- pypi: <pypi.org/project/jupyter/>

<br>

Jupyter notebook是一种Web应用，能让用户将说明文本、数学方程、代码和可视化内容全部组合到一个易于共享的文档中。

**安装**

```
#bash
sudo pip3 install jupyter


#Anaconda
conda install jupyter


#运行
jupyter notebook --no-browser --ip=0.0.0.0
#建议先设置密码
jupyter notebook password

jupyter notebook --no-browser --ip=192.168.31.119 --notebook-dir=/tmp/notebook

```

打开浏览器访问，输入密码：

![](/images/Python/jupyter_login.png)

Anaconda虚拟环境目录：

![虚拟环境](/images/Python/jupyter_dash.png)

栗子：

![栗子](/images/Python/jupyter_test.png)

<br>











