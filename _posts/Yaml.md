---

title: Yaml
date: 2017-09-02 17:16:38
categories: DevOps
tags:
- Yaml

---


参考:

- 维基百科: <https://zh.wikipedia.org/wiki/YAML>
- YAML: <https://yaml.org/>
- YAML语法检测: <http://www.yamllint.com>



<br/>
<br/>

<!--more-->

---

<br/>
<br/>



# 概述


YAML: YAML Ain't Markup Language.
YAML is a human friendly data serialization standard for all programming languages.

<br>

YAML（`/ˈjæməl/`，尾音类似camel骆驼）是一个可读性高，用来表达数据序列的格式。YAML是"YAML Ain't a Markup Language"(YAML不是一种标记语言)的递归缩写。在开发的这种语言时，YAML的意思其实是："Yet Another Markup Language"（仍是一种标记语言，但为了强调这种语言以数据做为中心，而不是以标记语言为重点，而用反向缩略语重命名。

YAML是专门用来写配置文件的语言，非常简洁和强大，远比JSON格式方便。

<br>

YAML的特点:

- YAML数据可在编程语言之间移植
- 包括数据一致的数据模型
- 人类易于阅读
- 支持单向处理
- 易于实现和使用




<br/>
<br/>

---

<br/>
<br/>




# 语法规则


基本语法规则如下：

- 大小写敏感
- 使用缩进表示层级关系
- 缩进时不允许使用`Tab`键，只允许使用空格
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可(一般2个或4个)
- `#`表示单行注释，不支持多行注释。多行注释请使用多个`#`
- 具有单个流的多个文档用3个连字符(`---`)分隔
- 阻止列表项包括与周围块级相同的缩进，因为`-`符号被视为缩进的一部分



<br/>
<br/>

---


<br/>
<br/>



# 格式


YAML支持三种格式：

- **对象**：键值对的集合，又称为映射(mapping)/哈希(hashes)/字典(dictionary)
- **数组**：一组按次序排列的值，又称为序列(sequence)/列表(list)
- **纯量值**（scalars）：单个的、不可再分的值

```yaml
#栗子


- key1: value1
- key2: value2

#一个列表，两个对象，四个纯量
```



<br/>
<br/>



## 对象


对象是一组键值对，使用冒号结构表示。

```yaml
key: vaule


#Yaml也允许另一种写法，将所有键值对写成一个行内对象。
hash: { name: Steve, foo: bar }
```


<br/>
<br/>



## 数组

又称为列表或序列。

```yaml
- list
- sequence
- array


#数组也可以采用行内表示法。
animal: [list, sequence, array]
```

<br>

数据结构的子成员是一个数组，则可以在该项下面缩进一个空格。

```yaml
-
 - Cat
 - Dog
 - Goldfish
```


<br/>
<br/>



## 纯量值

纯量是最基本的、不可再分的值。

－　字符串
－　布尔值
－　整数
－　浮点数
－　Null
－　时间
－　日期




<br/>
<br/>



## 复合结构

对象和数组可以结合使用，形成复合结构。

```yaml
languages:
 - Ruby
 - Perl
 - Python
websites:
 YAML: yaml.org
 Ruby: ruby-lang.org
 Python: python.org
 Perl: use.perl.org
```



<br/>
<br/>

---

<br/>
<br/>



# 特殊符号


- `---`: 表示文档开始；分割不同内容
- `...`和`---`的配合使用: 在一个配置文件中代表一个文件的结束
- `!!`: 类型强行转换
- `>`: 在字符串中折叠换行
- `|`: 保留换行符
- 引用
	- `&`: 完成锚点定义
	- `*`: 完成锚点引用
- 合并内容: 主要和锚点配合使用，可以将一个锚点内容直接合并到一个对象中

<br>

```yaml
#栗子

---
Time: 2018-07-17T15:02:31+08:00
User: ed
Warning:
     This is an error message for the log file
---
Time: 2018-07-17T15:05:21+08:00
User: ed
Warning:
    A slightly different error message.
```

<br>

```yaml
#栗子
---
time: 20:03:20
player: Sammy Sosa
action: strike (miss)
...
---
time: 20:03:47
player: Sammy Sosa
action: grand slam
...
```

<br>

```yaml
string:
    - !!str 54321
    - !!str true
```







