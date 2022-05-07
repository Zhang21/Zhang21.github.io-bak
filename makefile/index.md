# Makefile



参考:

- <https://seisman.github.io/how-to-write-makefile>
- <http://www.ruanyifeng.com/blog/2015/02/make.html>


<br>

<!--more-->

<br>


## 概述

代码编程可执行文件，叫做编译(compile)。先编译这个，再编译那个，叫做构建(build)。

`make` 是常用的构建工具。构建规则都写在 `makefile` 里，要学会如何make命令，就必须学会编写makefile文件。



<br>

---

<br>



## Makefile文件格式

<br>

### 概述

Makefile文件由一系列规则构成，每条规则的形式如下：

- 目标(target)：必须
- 前置条件(prerequisites)： 可选，但前置条件和命令必须至少存在一个。
- tab
- 命令(commands)：可选，但前置条件和命令必须至少存在一个。

```makefile
<target>: <prerequisites>
[tab] <commands>
```

每条规则就明确两件事，构建目标的前置条件是什么，以及如何构建。


<br>
<br>


### 目标

目标通常是文件名（多个文件之间用空格分隔），指明make命令所要构建的对象。目标还可以是某个操作的名字，这称为 **伪目标** (phony target)。

```makefile
clean:
	rm *.o
```

上面代码的目标是clean，它不是文件名，而是一个操作的名字，属于伪目标。

```bash
make clean
```

但是，如果在当前目录中，正好有一个文件叫做clean，那么这个命令不会执行。因为make发现clean文件已存在，就认为没有必须要重新构建了，就不会执行rm命令。

为了避免这种情况，可以明确声明clean是伪目标。声明伪目标之后，make就不会去检查是否存在一个叫做clean的文件。

```makefile
# 为声明伪目标
.PHONY: clean
clean:
	rm *.o temp
```

<br>

如果make命令运行时没有指定目标，默认会执行Makefile文件的第一个目标。

```bash
make
```


<br>
<br>


### 前置条件

前置条件通常是一组文件名，用空格分隔。它指定了目标是否需要重新构建的判断标准，只要有一个前置条件不存在，或者有过更新，目标就需要重新构建。

```makefile
result.txt: source.txt
	cp source.txt result.txt
```


<br>
<br>


### 命令

命令表示如何更新目标，它是构建目标的具体指令。

每行命令之前必须有tab键，如果想用其它键，可以使用内置的 `.RECIPEPREFIX` 来声明。

```makefile
# 使用 > 替换 tab
.RECIPEPREFIX = >
all:
> echo Hello, world
```

<br>

需要注意的是，每行命令在一个单独的shell中执行，这些shell之间没有继承关系。不过可以多个命令写在一行，或使用反斜线(`\`)，或使用 `.ONESHELL` 命令。

```makefile
var-lost:
    export foo=bar
    echo "foo=[$$foo]"
```

```makefile
var-kept:
    export foo=bar; echo "foo=[$$foo]"
```

```makefile
.ONESHELL:
var-kept:
    export foo=bar; 
    echo "foo=[$$foo]"
```



<br>

---

<br>



## Makefile文件语法

<br>

### 注释

```makefile
# 这里是注释
result.txt: source.txt
    # 这是注释
    cp source.txt result.txt # 这也是注释
```


<br>
<br>


### 回声

默认情况下，make会答应每条命令，然后再执行，这叫做回声(echoing)。

可以在命令前加上 `@` ，来关闭回声。

由于在构建过程中，需要了解当前执行详情，所以通常只在纯注释和显示的echo命令前面机上`@`。

```makefile
test:
    @# 这是测试
    @echo TODO
```


<br>
<br>


### 通配符

Makefile的通配符和Bash一直，主要有 `*`, `?`, `[...]`。


<br>
<br>


### 模式匹配

make命令允许对文件名，进行类似正则运算的匹配，主要用到的匹配符是`%`。

使用匹配符，可以将大量同类型的文件，只用一条规则就完成构建。

```makefile
%.o: %.c
```


<br>
<br>


### 变量和赋值

Makefile允许使用等号来自定义变量。

调用shell变量，需要在美元符号前多加一个美元符号。

```makefile
txt = Hello World
test:
    @echo $(txt)

test2:
    @echo $$HOME
```

<br>

makefile还提供了四个赋值运算符:

```
VARIABLE = value
# 在执行时扩展，允许递归扩展。

VARIABLE := value
# 在定义时扩展。

VARIABLE ?= value
# 只有在该变量为空时才设置值。

VARIABLE += value
# 将值追加到变量的尾端。
```


<br>
<br>


### 内置变量

make提供了一系列内置变量，比如， `$(CC)` 指向当前使用的编译器， `$(MAKE)` 指向当前使用的make工具。

其它内置变量，请参考[文档](https://www.gnu.org/software/make/manual/html_node/Implicit-Variables.html)


<br>
<br>


### 自动变量

make命令还提供一些自动变量，它们的值与当前的规则有关。

```makefile
# $@ 指代当前目标

a.txt b.txt:
    touch $@
# 等价于
a.txt:
    touch a.txt
b.txt:
    touch b.txt
```

<br>

```makefile
# $< 指代第一个前置条件

a.txt: b.txt c.txt
    cp $< $@
# 等价于
a.txt: b.txt c.txt
    cp b.txt a.txt
```

<br>

```makefile
# $? 指代比目标更新的所有前置条件， 以空格分隔。
```

<br>

```makefile
# $^ 指代所有前置条件，之间以空格分隔。
```

<br>

```makefile
# $* 指代匹配符 % 匹配的部分
```

<br>

```makefile
# $(@D) 和 $(@F) 分别指向 $@ 的目录名和文件名
```

<br>

```makefile
# $(<D) 和 $(<F) 分别指向 $< 的目录名和文件名
```


<br>
<br>


### 判断和循环

Makefile使用Bash语法，完成判断和循环。

```makefile
LIST = one two three
all:
    for i in $(LIST); do \
        echo $$i; \
    done
# 等同于
all:
    for i in one two three; do \
        echo $i; \
    done
```


<br>
<br>


### 函数

Makefile还可以使用函数。

```makefile
$(function arguments)
# 或者
${function arguments}
```

<br>

```makefile
# shell 函数用来执行 shell 命令
srcfiles := $(shell echo src/{00..99}.txt)
```

<br>

```makfile
# wildcard 函数用来在 Makefile 中，替换 Bash 的通配符。
srcfiles := $(wildcard src/*.txt)
```

<br>

```makefile
# subst 函数用来文本替换
$(subst from,to,text)
```

<br>

```makefile
# patsubst 函数用于模式匹配的替换
$(patsubst pattern,replacement,text)
```

<br>

```makefile
# 替换后缀名函数的写法是：变量名 + 冒号 + 后缀名替换规则
min: $(OUTPUT:.js=.min.js)
```



<br>

---

<br>




## Makefile实例

```makefile
# 执行多个目标
.PHONY: cleanall cleanobj cleandiff

cleanall : cleanobj cleandiff
        rm program

cleanobj :
        rm *.o

cleandiff :
        rm *.diff
```
















































