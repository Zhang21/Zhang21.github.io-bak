# Markdown



参考:

- [Markdown-wiki](https://zh.wikipedia.org/wiki/Markdown)
- [Markdown官网](https://daringfireball.net/projects/markdown/)
- [Markdown中文文档](https://markdown-zh.readthedocs.io/en/latest/)
- [Markdown语法](http://itmyhome.com/markdown/article/extension/strikethrougn.html)
- [果冻虾仁](https://github.com/guodongxiaren/README)



<br/>

<!--more-->

<br/>

---

<br>


# 关于


Markdown 是一种轻量级标记语言。
它允许人们使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML(或者HTML)文档。



<br/>
<br>

---

<br>


# 语法

<br>

## 首行缩进

```
#一个空格
&ensp;

#两个空格
&emsp;

#不断行空白格
&nbsp;
```

**栗子：**

1. &ensp;一个空格；
2. &emsp;两个空格；
3. &nbsp;不断行空白格；


<br>
<br>


## 段落与换行

- 段落的前后必须是空行
 	* 空行是指行内什么都没有，或者只有空白符（空格或制表符）
 	* 相邻两行文本，如果中间没有空行，会显示在一行中（换行符被转换为空格）
- 如果需要在段内加入换行
	* 可以在前一行的末尾加入至少两个空格，然后换行写其它的文字
- Markdown中的多数区块都需要在两个空行之间


<br>
<br>


## 粗体和斜体

语法：

```
*斜体*, _斜体_
**粗体**
***粗斜体***
~~删除线~~
```

显示效果：

- *斜体*, _斜体_
- **粗体**
-  ***粗斜体***
- ~~删除线~~


<br>
<br/>

## 分级标题

### Setext形式

大标题：

```
一级大标题
========


二级大标题
--------
```


<br>
<br>

### atx形式

普通标题：

```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
```


<br/>
<br>


## 超链接

MarkDown支持两种形式的链接语法：行内式和参考式。

<br>

### 行内式

语法说明：
[ ] 里面写链接文字，( ) 里面写链接地址，()中的" "可以指定title属性。

代码：

`欢迎来到 [简书](www.jianshu.com "Jianshu")`

效果：

欢迎来到 [简书](www.jianshu.com "Jianshu")

<br>

### 参考式

参考式超链接一般用在学术论文上面，或某一个链接在文章中多处使用，那么引用的方式创建链接将非常好，它可以让你对链接进行统一的管理。

语法说明：

```
参考式链接分为两部分，文中的写法[链接文字][链接标记]，在文本任意位置添加[链接标记]:链接地址 “链接标题”，链接地址与链接标题前有一个空格
如果链接文字本身可以作为链接标记，也可以写成[链接文字][]
[链接文字]：链接地址的形式
```


代码：

```
简书里面有 [简书早报][1]、[简书晚报][2]以及 [简黛玉][3]
[简黛玉 美人][3] 是一个[才女][]
[1]:http://www.jianshu.com "Jianshu"
[2]:http://www.jianshu.com "EveningPaper"
[3]:http://www.jianshu.com
[才女]:http://www.jianshu.com
```

效果：

简书里面有 [简书早报][1]、[简书晚报][2]以及[简黛玉][3]
[简黛玉 美人][3] 是一个[才女][]
<br/>
[1]:http://www.jianshu.com "Jianshu"
[2]:http://www.jianshu.com "EveningPaper"
[3]:http://www.jianshu.com
[才女]:http://www.jianshu.com


### 自动链接

MarkDown支持以比较简短的自动链接形式来处理网址和电子邮件，只要用<>包起来，MarkDown就会自动把它转成链接。

代码：

```
<http://example.com>
<address@example.com>
```


<br>
<br>


## 锚点

MarkDown Extra只支持在标题后插入锚点，其他地方无效。
锚点中的标题如果有空格，则锚点无效。
现在的锚点支持中文标题。


代码：

```
锚点连接页内标题

[标题一](#Title1)
[标题二](#Title2)
[标题三](#标题3)


# Title1
## Title2
### 标题3
```


<br>
<br/>


## 列表

### 无序列表

使用 `* ，+ ，- ` 表示无序列表

代码：

```
- 无序列表1
- 无序列表2
- 无序列表3
```


效果：
- 无序列表1
- 无序列表2
- 无序列表3

<br/>

### 有序列表

有序列表使用数字接着英文点


代码：

```
1. 有序列表1
2. 有序列表2
3. 有序列表3
```


效果：

1. 有序列表1
2. 有序列表2
3. 有序列表3

<br>

### 定义型列表

定义型列表由名词和解释组成。一行写上定义，紧跟一行写上解释。解释的写法：紧跟一个缩进（Tab）

<br>

### 列表缩进

列表项目标记通常是放在最左边，但是其实也可以缩进，最多3个空格，项目标记后则一定要接着至少一个空格或制表符。

代码：

```
*    轻轻的我走了， 正如我轻轻的来； 我轻轻的招手， 作别西天的云彩。
那河畔的金柳， 是夕阳中的新娘； 波光里的艳影， 在我的心头荡漾。
*    那榆荫下的一潭， 不是清泉， 是天上虹； 揉碎在浮藻间， 沉淀着彩虹似的梦。
```


效果：

*    轻轻的我走了， 正如我轻轻的来； 我轻轻的招手， 作别西天的云彩。
那河畔的金柳， 是夕阳中的新娘； 波光里的艳影， 在我的心头荡漾。

*    那榆荫下的一潭， 不是清泉， 是天上虹； 揉碎在浮藻间， 沉淀着彩虹似的梦。



<br>
<br>


## 引用

引用需要在被引用的文本前加上`>`符号

代码：

```
> 引用1
> 引用2
```

效果：

> 引用1
> 引用2

<br>

### 引用的多层嵌套

区块引用可以嵌套（如引用的引用），只要根据层次加上不同数量的 ` > `符号

代码：

```
>>> 请问MarkDown怎么用？
>> 自己看教程！
> 教程在哪里？
```

效果：

> 请问MarkDown怎么用？
>> 自己看教程！
>>> 教程在哪里？



<br>
<br>

## 插入图像

图片的创建方式与超链接类似。

代码：

`![](http://zhangxx5678.lofter.com/post/39b969_df4f526#)`


<br>
<br>


## 内容目录

在段落中填写 `[TOC]` 以显示全文内容结构目录


<br>
<br/>


## 注脚

在需要添加注脚的文字后加上注脚名字 [^注脚名字]，称为加注。然后在文中的任意位置（一般最后）添加脚注，脚注前必须有对应的脚注名字。
注脚与注脚间必须空一行！
注脚自动被搬运到最后面，请到文章末尾查看，并且脚注后的链接可以直接跳转会到加注的地方


代码：

```
使用 MarkDown[^1]可以提高书写效率，直接转换成 HTML[^2]
[^1]:MarkDown是一种纯文本标记语言
[^2]:HTML超文本标记语言
```


效果：

 使用 MarkDown[^1]可以提高书写效率，直接转换成 HTML[^2]
[^1]:MarkDown是一种纯文本标记语言
[^2]:HTML超文本标记语言


<br/>
<br>

## 分割线

可以在一行中用 三个以上的 `*`,`-`,`_` 建立一个分割线，行内不能有其他东西。

代码：

```
1. * * *
2.
3. ***
4.
5. - - -
6.
7. ---
```

效果：

* * *

***

___

---



<br>
<br>

## GitHub中的表情

Github的Markdown语法支持添加emoji表情，输入不同的符号码（两个冒号包围的字符）可以显示出不同的表情。

比如`:blush:`,显示效果为 :blush:

<br>

每个表情对应的符号码：<https://www.webpagefx.com/tools/emoji-cheat-sheet/>

或者，果冻虾仁的整理：<https://github.com/guodongxiaren/README/blob/master/emoji.md>



<br>
<br>

## Diff语法

版本控制系统中都少不了`diff`功能——展示一个文件内容的增加与删除。

- 绿色(+)表示新增
- 红色(-)表示删除

语法效果与代码高亮类似，在三个反引号后面写上diff。在内容中`+`表示新增，`-`表示删除。

```diff
+ 111
+ 11
+ 1
- 222
- 22
- 2
```




<br>

---

<br>



# 扩展语法

Markdown标准 本身所包含的功能有限，所以产生了许多第三方扩展语法，如 [GFW, GitHub Flavored Markdown](https://help.github.com/categories/writing-on-github/)


## Tasklist

代码：

```
- [ ] Monday
- [ ] Tuesday
- [ ] Wednesday
- [ ] Tuesday
- [ ] Friday
```


效果：

- [ ] Monday
- [ ] Tuesday
- [ ] Wednesday
- [ ] Tuesday
- [ ] Friday


<br>
<br/>

## 表格

1. 不管是哪种方式，第一行为表头，第二行为分割表头和主体部分，第三行开始每一行为一个表格行
2. 列与列之间用管道符号 | 隔开
3. 还可设置对齐方式
	+ 左对齐 `:|`
	+ 右对齐 `|:`
	+ 中对齐 `:|:`



代码：

```
学号 | 姓名 | 分数
- | - | -
001 | 张三 | 78
002 | 李四 | 67
003 | 王五 | 99
```


学号 | 姓名 | 分数
- | - | -
001 | 张三 | 78
002 | 李四 | 67
003 | 王五 | 99


<br>
<br/>


## GitHub上的表格

GitHub上的表格与上有一点不同。

```md
| 学号 | 姓名 | 分数
| - | - | -
| 001 | 张三 | 78
| 002 | 李四 | 67
| 003 | 王五 | 99
```

| 学号 | 姓名 | 分数
| - | - | -
| 001 | 张三 | 78
| 002 | 李四 | 67
| 003 | 王五 | 99


<br>
<br>


## 代码块和高亮

### 代码块

插入代码的方式有两种，一种是利用缩进(Tab)，另一种是利用反引号 \`\` 和 \`\`\`     \`\`\`

代码：

```
Python语言的输出函数 `Print()` 怎么使用？
```


效果：

Python语言的输出函数 `Print()` 怎么使用？

```py
import os
from flask import Flask
app = Flask(app)
```


<br>


### 高亮

在 \`\`\` 之后添加代码的语言


代码：

\`\`\`python
import os
from flask import Flask
app = Flask(app)
\`\`\`


效果：

```python
import os
from flask import Flask
app = Flask(app)
```


<br>
<br/>



## 流程图

flow chart/flow diagram，需要安装额外的插件才能支持流程图。

流程图语法参考: <https://mermaidjs.github.io/>
Hexo Plugins: <https://hexo.io/plugins/>

<br>

hexo默认好像不支持流程图，需要在hexo Plugins去查找此类插件，安装此类插件，然后修改hexo配置文件。
具体的使用方法请参考插件说明。



<br>
<br>



## 数学公式

参考:

- <https://blog.csdn.net/lanxuezaipiao/article/details/44341645>
- <https://juejin.im/post/5a6721bd518825733201c4a2>
- [LaTex数学符号表](https://github.com/mk43/BlogResource/blob/master/LaTex/LATEX%E6%95%B0%E5%AD%A6%E7%AC%A6%E5%8F%B7%E8%A1%A8.pdf)

- **LaTEX:** <https://zh.wikipedia.org/wiki/LaTeX>
是一种跨平台的基于[TEX](https://zh.wikipedia.org/wiki/TeX)的排版系统，对于生成复杂表格和数学公式，这一点表现得尤为突出。
因此它非常适用于生成高印刷质量的科技和数学、化学类文档。

- **MathJax**: <https://en.wikipedia.org/wiki/MathJax>
MathJax是一种跨浏览器JavaScript库，它使用MathML，LaTeX和ASCIIMathML 标记在Web浏览器中显示数学符号。
MathJax作为Apache License下的开源软件。

- **MathJax语法:** <https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference>

<br>

由于许多支持markdown的都不会显示LaTeX公式，所以有一个加载链接来显示的效果。

参考知乎: <https://www.zhihu.com/question/26887527>

```
#如
![](http://latex.codecogs.com/gif.latex?\\frac{1}{1+sin(x)})

#后接具体的公司，注意转移
```

<br>
<br/>


### 公式

许多扩展的Markdown编辑器支持基于Mathjax编写的数学公式。

LaTeX有两种数学公式：

- **行内式**
与其它文字混杂。
这是一个`$行内式$`栗子

- **块级公式**
单独成行。
`$$块级公式$$`


栗子：

```
这是一个$E=mc^2$公式


$$\sum_{i=1}^n a_i=0$$

$$f(x_1,x_x,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2 $$

$$\sum^{j-1}_{k=0}{\widehat{\gamma}_{kj} z_k}$$
```


<br/>
<br/>


### 基本类型

- 上/下标
- 分式
- 根式
- 求和
- 积分
- 矩阵
- 数组
- 空格
- 省略号
- 矢量
- 括号
- 希腊字母



<br/>


#### 上下标

- 上标(superscript): `^`
- 下标(subscript): `_`

如果上小标内容大于一个字符，要用`{}`
虽然空格和顺序对公式没有影响，但建议使用统一的风格，不用混用：

- 先下标后上标
- 先上标后下标

```
#上标
$$E=mc^2$$


#下标
$$x_2$$


#上下标
$$x_{subscript} ^{superscript}$$


#左右上下标
$${}_{a} ^{b} x ^{c} _{d}$$

$${}_{左下} ^{左上} x ^{右上} _{右下}$$

#空格和顺序其实没影响
````

![subscript/superscript](/images/Markdown/super_sub_script.png)


<br/>


#### 分式

分式(fraction)
为了区分`frac`是函数不是公式，使用`\frac`进行转义


```
#用法
$$\frac{分子}{分母}$$


$$\frac{x+y} {2}$$

$$\frac{1} {1+\frac{1} {2}}
```

![fraction](/images/Markdown/fraction.png)


<br/>


#### 根式

开方(sqrt)
使用`\sqrt`转义

```
#默认为开平方
$$\sqrt[开方次数]{开方因子}$$


$$\sqrt {x}$$

$$\sqrt[3] {\frac{x} {y}}$$

$$\sqrt[x] {1+\sqrt[y] {1+a^2}}$$
```

![sqrt](/images/Markdown/sqrt.png)



<br/>


#### 求和

求和(summation)
使用`\sum`转义

```
#用法
$$\sum_{起点}^{终点}表达式$$


$$\sum_{i=0} ^{n} \frac{1} {k}$$

$$\sum_{i=0}^{n} i^2=\frac{(n^2+n)(2n+1)} {6}$$
```

![summation](/images/Markdown/summation.png)


<br/>


#### 积分

积分(integral)
使用`\int`转义

```
#用法
$$\int_{下限}^{上限} 被积函数d被积量$$


$$\int_{a}^{b} f(x)dx$$
```

![integral](/images/Markdown/integral.png)


<br/>


#### 矩阵

矩阵(matrix)

```
#&区分行间元素
#\\\\代表换行


#无括号矩阵
$$\begin{matrix}1 & 2 \\\\ 3 & 4 \end{matrix}$$


#花括号矩阵
$$\begin{pmatrix} 1&2 \\\ 3&4 \end{pmatrix}$$


#中括号矩阵
$$\begin{bmatrix} 1&2 \\\\ 3&4 \end{bmatrix}$$


#大括号矩阵
$$\begin{Bmatrix} 1&2 \\\\ 3&4 \end{Bmatrix}$$


#竖线矩阵
$$\begin{vmatrix} 1&2 \\\\ 3&4 \end{vmatrix}$$


#双竖线矩阵
$$\begin{Vmatrix} 1&2 \\\\ 3&4 \end{Vmatrix}$$
```

![matrix](/images/Markdown/matrix.png)


<br/>
<br/>


#### 数组

数组(array)


<br/>
<br/>


#### 空格

空格(space/blank)

```
#紧贴
$$a\!b$$


#正常
$$ab$$


#小空格
$$a\,b$$


#中空格
$$a\;b$$


#大空格
$$a\ b$$


#quad空格
$$a\quad b$$

#两个quad空格
$$a\qquad b$$
```

![space](/images/Markdown/space.png)


<br/>
<br/>


#### 省略号

省略号(ellipsis)

- `\ldots`
与文本底线对齐的省略号

- `cdots`
与文本中线对齐的省略号

```
#\ldot, \cdot可表示单个点，对齐方式不变


$$f(x_1,x_2,\ldots,x_n) = x_1^2 + x_2^2 + \cdots + x_n^2$$
```

![ellipsis](/images/Markdown/ellipsis.png)


<br/>
<br/>


#### 矢量

矢量(vector)

```
#用法
$$\vec{矢量值}$$


$$\vec{a}$$

$$\vec{a} \cdot \vec{b}=0$$
```

![vector](/images/Markdown/vector.png)


<br/>
<br/>

#### 括号

括号(bracket)

`()`, `[]`, `|`可以直接表示，而`{}`本来是用于分组，因此需要转义`\{\}`来表示，也可使用`\lbrace`, `\rbrace`来表示。

原始括号并不会随着公式大小缩放，需要使用`\left`和`\right`标记实现自适应调整，它两必须成对出现。

```
#小括号()
$$(\frac{1}{2})$$
$$\left( \frac{1}{2} \right)$$


#中括号[]
$$[\frac{1}{2}]$$
$$\left[ \frac{1}{2} \right]$$


#绝对值|
$$|\frac{1}{2}|$$
$$\left| \frac{1}{2} \right|$$


#大括号{}
$$\{ \frac{1}{2} \}$$
$$\left\{ \frac{1}{2} \right\}$$


#尖括号<>
#\langle  \rangle
$$\langle \frac{1}{2} \rangle$$
$$\left\langle \frac{1}{2} \right\rangle$$


#向正无穷大取整(ceil)
$$\lceil \frac{1}{2} \rceil$$
$$\left\lceil \frac{1}{2} \right\rceil$$


#向下取整
$$\lfloor \frac{1}{2} \rfloor$$
$$\left\lfloor \frac{1}{2} \right\rfloor$$

```

![bracket](/images/Markdown/bracket.png)


<br/>
<br/>


### 希腊字母


| 大写 | LaTex代码 | 小写 | LaTex代码 | 中文名称 |
| - | - | - | - |
| A | A | α | `\alpha` | 阿尔法 |
| B | B | β | `\beta` | 贝塔 |
| Γ | Γ | γ | `\gamma` | 伽马 |
| D | D | δ | `\delta` | 德尔塔 |
| E | E | ϵ | `\epsilon` | 伊普西隆 |
| Z | Z | ζ | `\zeta` | 泽塔 |
| H | H | η | `\eta` | 伊塔 |
| Θ | Θ	| θ	| `\theta` | 西塔 |
| I	| I	| ι	| `\iota` | 约塔 |
| K	| K	| κ	| `\kappa` | 卡帕 |
| Λ	| Λ	| λ	| `\lambda`	| 兰姆达 |
| M	| M	| μ	| `\mu` | 缪 |
| N	| N	| ν	| `\nu`	| 纽 |
| X	| X	| ξ	| `\xi`	| 克西 |
| O	| O	| ο	| `\omicron` | 欧米克隆 |
| P	| P	| π	| `\pi` | 派 |
| R	| R	| ρ	| `\rho` | 柔 |
| Σ	| Σ	| σ	| `\sigma`	| 西格玛 |
| T	| T	| τ	| `\tau` | 陶 |
| Υ	| Υ	| υ	| `\upsilon` | 宇普西隆 |
| Φ	| Φ	| ϕ	| `\phi` | 弗爱 |
| X	| X	| χ	| `\chi` | 卡 |
| Ψ	| Ψ	| ψ	| `\psi` | 普赛 |
| Ω	| Ω	| ω	| `\omega`	| 欧米伽 |

<br>

```
$$\alpha$$

$$\beta$$

$$\gamma$$

$$\delta$$
```

![](/images/Markdown/alpha.png)


<br/>
<br/>


### 特殊符号

- 关系运算符
- 集合运算符
- 对数运算符
- 三角运算符
- 微积分运算符
- 逻辑运算符
- 带帽符号
- 连线符号
- 箭头符号


<br/>


#### 关系运算符

```
±：\pm 
×：\times 
÷：\div 
∣：\mid 
∤：\nmid 
⋅⋅：\cdot 
∘：\circ 
∗：\ast 
⨀：\bigodot 
⨂：\bigotimes 
⨁：\bigoplus 
≤：\leq 
≥：\geq 
≠：\neq 
≈：\approx 
≡：\equiv 
∑：\sum 
∏：\prod 
∐：\coprod
```

<br/>


#### 集合运算符

```
∅ ：\emptyset 
∈：\in 
∉：\notin 
⊂：\subset 
⊃：\supset 
⊆：\subseteq 
⊇：\supseteq 
⊇：\bigcap 
⋃：\bigcup 
⋁：\bigvee 
⋀：\bigwedge 
⨄：\biguplus 
⨆：\bigsqcup
```


<br/>


#### 对数运算符

```
log：\log 
lg：\lg 
ln：\ln
```


<br/>


#### 三角运算符

```
⊥ ：\bot 
∠：\angle 
30∘：30^\circ 
sin：\sin 
cos：\cos 
tan：\tan 
cot：\cot 
sec：\sec 
csc：\csc
```


<br/>


#### 微积分运算符

```
′：\prime 
∫：\int 
∬：\iint 
∭：\iiint 
∬∬：\iiiint 
∮：\oint 
lim：\lim 
∞：\infty 
∇：\nabla
```


<br/>


#### 逻辑运算符

```
∵ ：\because 
∴：\therefore 
∀：\forall 
∃：\exists 
≠：\not= 
≯：\not> 
⊄：\not\subset
```


<br/>


#### 戴帽符号

```
y^ ：\hat{y} 
yˇ：\check{y} 
y˘：\breve{y}
```


<br/>


#### 箭头符号

```
↑ ：\uparrow 
↓：\downarrow 
⇑：\Uparrow 
⇓：\Downarrow 
→：\rightarrow 
←：\leftarrow 
⇒：\Rightarrow 
⇐：\Leftarrow 
⟶：\longrightarrow 
⟵：\longleftarrow 
⟹：\Longrightarrow 
⟸：\Longleftarrow
```
















<br/>
<br/>

---

<br>



# 生成GitHub上的徽章


- 生成徽章: <https://shields.io/>
- 生成进度： <https://github.com/fehmicansaglam/progressed.io>

<br>

进入徽章(Badge)网站，找到如下部分生成徽章。之后会得到一个徽章地址，在GitHub中插入此徽章地址就好。

![Badge](/images/MakeBadge.png)

<br>

进入进度(Progress)网站，找到某个格式的链接，在GitHub中插入此链接。


<br/>
<br/>

---

<br/>



# 编辑器

介绍一些常用的书写、编辑Markdown的工具。

- MarkdownPad Windows (windows);
- Texts (Windows, osX);
- MarkPad (Windows);
- Haroopad (Windows, osX, Linux);
- ReText (Linux);
- 等等



<br/>

---

<br>




# 格式转换


Markdown文档可以方便地转换为 HTML, Word, PDF 等文件格式。
可以利用 软件 或者 命令 转换文件。

- 转换为 HTML
- 转换为 PDF
- 转换为 Word
















