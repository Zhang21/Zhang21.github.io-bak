---
title: HTML CSS
date: 2019-10-11 11:59:21
mathjax: true
categories: Frontend
tags:
  - Frontend
  - HTML
  - CSS
---



参考:

- 《Head First HTML and CSS》
- w3cSchool: <https://www.w3school.com.cn/>





<br/>
<br/>

---

<!--more-->

<br/>
<br/>



















# 认识HTML

HTML(Hyper Text Markup Language)超文本标记语言，创建结构。
HTML会告诉浏览器文档的结构，标题放在哪、段落放在哪、哪些文本需要强调...

CSS(Cascading Style Sheets)层叠样式表，创建样式。

与试图使用一种语言兼顾这两方面的工作相比，实际上学习两种语言让它们各司其职反而更容易。

- 首部(`head`): 包含了web页面的有关信息
- 页面元素(`body`): 包含web页面的所有内容和结构
- 元素(element): `开始标记+内容+结束标记`（如`element=<>+content+</>`），某些例外
- 属性(attribute): 如`type="text/css"`，能提供元素的一些额外信息
- `<Style>`元素放在HTML首部

<br>

```html
<!--index.html-->

<!DOCTYPE html>
<html lang='en'>
<head>
    <meta charset="UTF-8">
    <title>Starbuzz Coffee</title>
    <style type="text/css">
        body {
            background-color:skyblue;
            margin-left: 20%;
            margin-right: 20%;
            border: 2px dotted black;
            padding: 10px 10px 10px 10px;
            font-family: sans-serif;
        }
    </style>
</head>
<body>
  <h1>Starbuzz Coffee Beverages</h1>
  <h2>House Blend</h2>
  <p>ABC</p>
</body>
</html>
```









<br/>
<br/>

---

<br/>
<br/>





















# 深入了解超文本

想从一个页面链接到另一个页面，使用`<a>`元素。
`<a>`元素的`href`属性制订了链接的目标文件。

文字和图像都可以用作链接的标签。
单击一个链接时，浏览器会加载`href`属性中指定的Web页面。

可以链接到相同文件夹中的文件，也可以链接到其它文件夹的文件。

请注意相对路径和绝对路径。

为网站选择的文件名和文件夹不要有空格。

<br>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>AAA</title>
    <style type="text/css">
        body {
            margin-left:20%;
            margin-right:20;
        }
    </style>
</head>
<body>
    <h1 id="top">Welcome</h1>
    <img src="images/aaa.jpg">
    <a href="aaa/aaa.html" title="aaa" target="_blank"> <h2>aaa</h2> </a>
    <p> abcd <em>aaa</em> </p>
    <a href='#top'><h3>返回TOP</h3></a>
</body>
</html>
```












<br/>
<br/>

---

<br/>
<br/>



































# 构建模块

开始输入内容之前要规划好Web页面的结构。首先画出一个草图，然后创建一个略图，最后再写出HTML。
规划页面时，首先设计大的块元素，然后用内联(inline)元素完善。

记住，要尽可能使用元素来告诉浏览器你的内容的含义。
一定要使用与内容含义最接近的元素。例如，如果需要一个列表，就不要使用段落元素。

`<p>, <blockquote>, <ol>, <ul>, <li>`都是块元素。它们单独显示，与内容前后分别有一个换行(默认的)。

`<q>, <em>`是内联元素。这些元素中的内容与其包含元素的其余内容放在一起。

需要插入自己的换行时，可以使用`<br>`元素，它是一个`void`元素。
`void`元素没有内容，只有一个标记组成。

空元素没有内容。不过它有开始和结束标记。

嵌套元素是指完全包含在另一个元素中的元素。如果元素能正确地嵌套，所有标记都能正确匹配。

要结合两个元素建立一个HTML列表，可以使用`<ol>`和`<li>`，也可以使用`<ul>`和`<li>`。

要对HTML内容中的特殊字符使用字符实体(character entity)，如`<`符号使用`&lt`。


<br/>


**元素大杂烩(elements)**

```
<a>        # 建立链接
<p>        # 段落
<q>        # 短引用
<blockquote> # 长引用
<code>     # 显示代码
<em>       # 斜体
<time>     # 告诉浏览器此内容是时间
<ul>       # 无序列表
<ol>       # 有序列表
<li>       # 列表
<strong>   # 强调文本
<pre>      # 浏览器按照你输入的格式显示文本
<br>       # 换行
<img>      # 图像
```

<br>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>333</title>
    <style type="text/css">
        body {
            margin-left: 20%;
            margin-right: 20%;
        }
        q, blockquote, ol, ul {
            font-size: x-large;
        }
    </style>
</head>
<body>
    <h1>333</h1>
    <p>333</p>

    <h2>333</h2>
    <img src="../images/333.png">
    <p>
        333333333333:
        <ol>
            <li>3</li>
            <li>33</li>
            <li>333</li>
        </ol>
    <p>
        3333
        <blockquote>
            3<br>
            33<br>
            333<br>
        </blockquote>
        333
    </p>

    <h2>333</h2>
    <p>
        333:
        <ul>
            <li>3</li>
            <li>33</li>
            <li>333</li>
        <ul>
        333
    </p>
</body>
</html>
```






















<br/>
<br/>

---

<br/>
<br/>





















# 连接起来

要把网站发布到Web上，通常最好的方法就是找一家公司来托管你的Web页面。
域名(domain)是一个唯一的名字，如`aaa.com`，用来唯一标识网站。
托管公司可能会为你的域创建一个或多个Web服务器，通常为`www`。

URL是统一资源定位符或Web地址，可用来标识Web上的任何资源。
定性的URL是由一个协议、一个网站名和资源的一个绝对路径组成。

HTTP是一个请求和响应协议，用来在Web服务器和浏览器之间传送Web页面。
浏览器使用`file:///`协议从你的计算机读取页面。

`index.html`和`default.html`都是默认页面，如果指定一个目录而没有指定文件名，则Web服务器会查找一个默认页面返回到浏览器。

`<a>`元素的`href`属性中可以使用相对路径或URL来链接其它web页面。对于你的网站中的其它页面，最好使用相对路径，对外部链接才使用URL。
可以用`id`属性在页面中创建一个目标（如`<h1 id="top">`），使用`#`后面加一个目标`id`（如`<a href="#top">`），可以链接到页面中的那个位置。
为了便于访问，可以在`<a>`元素中使用`title`属性提供链接的一个描述。
使用`target`属性在另一个窗口中打开链接。对于不同设备和浏览器，`target`属性可能会有问题。

<br>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>444</title>
    <style type="text/css">
        body {
            margin-left: 20%;
            margin-right: 20%;
        }
    </style>
</head>
<body>
    <h1 id="top">444</h1>
    <img src="images/444.jpg" width="800" height="400">
    <a href="about/444.html" title="444" taget="_blank"> <h2>4444</h2> </a>
    <p>
        4 <em>44</em>
        444
    </p>
    <a href="#top"> <h3>Top</h3></a>
</body>
</html>
```
















<br/>
<br/>

---

<br/>
<br/>



































# 为页面添加图像

使用`<img>`元素在web页面中添加图像。`<img>`元素是一个内联元素，这说明浏览器不会在图像前后插入一个换行。
浏览器对`<img>`元素的处理与其它HTML元素稍有不同。读取HTML页面之后，浏览器会从Web服务器获取各个图像并显示。
如果web页面上有多个大图像，则可以通过创建图像的缩略图使web页面更可用，下载也更快。缩略图是一些小图像（大图像的缩小版本），用户单击这些缩略图时可以看到原来的大图像。

要利用`src`属性指定图像文件的位置。可以在`src`属性中使用相对路径包含自己网站中的图像，或者可使用URL包含其它网站的图像。

`alt`属性是对图像的一个有意义的描述，在一些浏览器中，如果无法找到图像，就会显示这个描述。

对于浏览器来说，太大的图像会使web页面很难用，而且下载和显示都很慢。
图像可以用作指向其它web页面的链接。

```
JPEG、PNG和GIF是web浏览器广泛支持的3中图像格式
JPEG格式最适合保存照片和其他复杂图像
GIF或PNG格式最适合保存Logo和其他包含单色、线条或文本的简单图形
JPEG图像可以按不同的质量(quality）压缩，所以可以很好地权衡图像质量和文件大小，来满足你的需要
GIF和PNG图像格式允许建立一个有透明背景的图像。如果把一个有透明背景的图像放在一个web页面中，图像后面的东西（如页面背景色）就是透过图像的透明部分显示出来
GIF和PNG是无损格式，这说明相比JPEG文件，这些格式的文件往往更大
PNG可以提供比GIF更好的透明度控制，而且不像GIF只支持256中颜色，PNG可以支持更多颜色)
```

<br>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>555</title>
    <style type="text/css">
        body {
            background-color: rgb(228, 243, 191);
            margin-left: 10%;
            margin-right: 10%;
        }
    </style>
</head>
<body>
    <p><img src="logo/555.png"></p>

    <h1>555</h1>
    <p>
        <pre>
        5
        55
        555
        </pre>
    </p>

    <h2>55</h2>
    <p>
        5
        55
    </p>
    <p>
        <a href="html/5.html">
            <img src="5/5.jpg" alt="555">
        </a>
    </p>
</body>
</html>
```
































<br/>
<br/>

---

<br/>
<br/>































# CSS入门

CSS包含一些简单语句，成为规则。每个规则为选择的一些HTML元素提供样式。

定性的规则包括一个选择器，以及一个或多个属性和值。
选择器指定规则将应用到哪些元素。
每个属性声明以一个分号结束。
规则中的所有属性和值都放在`{}`大括号之间。

可以使用元素作为选择器，来选择任意元素。
通过用逗号分隔元素名，可以一次选择多个元素。

要在HTML包含一个样式，最容易的办法就是使用`<style>`标记。
对于HTML以及相当复杂的网站，可能要链接到一个外部样式表。`<link>`元素用于包含一个外部样式表。

很多属性都能继承。例如，如果为`<body>`元素设置了一个可继承的属性，那么`<body>`的所有子元素都会继承这个属性。
通过为你想改变的元素创建一个更特定的规则，能覆盖该元素继承的属性。

可以使用`class`属性将元素增加到一个类。
使用`.classname`可以选择属性这个类的多有元素。
通过在`class`属性中放入多个类名，可以指定一个元素属于多个类，类名之间用空格分隔。

通过在元素名和类名之间加一个`.`，可以选择该类中的一个特定元素。

<br>

**属性大杂烩**

```
color: 字体颜色
font-weight: 文本粗细
font-family: 字体样式
font-style: 斜体
font-size: 字体大小
left: 指定一个元素左边所在位置
top: 控制元素顶部的位置
line-height: 行间距
text-align: 文本对齐方式(左、右、居中)
letter-spacing: 字母之间设置间距
background-color: 背景色
list-style: 改变列表项外观
padding: 内边距
border: 外边距
```

<br>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>7</title>
    <link rel="stylesheet" href="css/7.css">
</head>

<body>
    <h1 id="top">77</h1>
    <img src="images/77.jpg">
    <a href="about/7.html" title="7" target="_blank"><h2>777</h2></a>
    <p>
        7 <em>77</em>
        777
    </p>
    <a href="#top"><h3>Top</h3></a>
    <p>
        Here <em>Tcons</em></a>
    </p>
</body>
</html>
```

```css
body {
    margin-left: 20%;
    margin-right: 20%;
}
h1, h2 {
    font-family: sans-serif;
    color: #6fa5fa;
}
h1 {
    text-decoration: underline;
}
p.icon {
    color: #4bd5ff;
}
```














<br/>
<br/>

---

<br/>
<br/>















































# 字体和颜色样式

访问者在你的web页面上看到的字体取决于他们自己计算机上安装了哪些字体，除非你使用web字体。

在`font-family`中指定候选字体是一个好主意，以防用户没有安装你的首选字体。
最后一个字体要指定为通用字体。这样一来，如果找不到其它字体，浏览器可以替换适当的字体。

如果你要使用某种字体，而默认情况下用户可能没有安装这种字体，可以在CSS中使用`@font-face`规则。

字体大小通常为： 像素(px)、斜体(em)、百分比(%)、关键字(small, mediuml...)
使用相对字体大小可以让你页面更可维护。

在body规则中使用字体大小关键字来设置基本字体大小，这样如果用户希望文本更大或更小，所有浏览器就能按比例缩放字体大小。

web颜色是混合数量不同的红、绿、蓝(RGB)得到的。
可以使用红绿蓝百分数指定你想要的颜色，也可以用红绿蓝数值(0-255)，或者使用颜色的十六进制码来指定颜色。

可以使用`text-decoration`属性为文本创建 上划线(overline)、下划线(underline)、中划线(line-through)。
有下划线的文档通常会被用户误以为是链接文件，所以要谨慎使用这个属性。

<br>

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>8</title>
    <link ref="stylesheet" href="../css/8.css">
</head>

<body>
    <h1>88</h1>
    <p>88!</p>

    <h2>888</h2>
    <img src="../images/8.jpg">
    <p>
        8888:
        <ol>
            <li>1</li>
            <li>2</li>
            <li>3</li>
        <ol>
    </p>

    <h2>8888</h2>
    <p>
        8888:
        <blockquote>
            88<br>
            888<br>
            8888<br>
        </blockquote>
        8888
    </p>
</body>

</html>
```

```css
@font-face {
    font-family: "Emblema One";
    src: url("http://wickedlysmart.com/hfhtmlcss/chapter8/journal/EmblemaOne-Regular.woff"),
         url("http://wickedlysmart.com/hfhtmlcss/chapter8/journal/EmblemaOne-Regular.ttf");
}

body {
    margin-left: 20%;
    margin-right: 20%;
    font-family: Verdana, Geneva, Arial, sans-serif;
    font-size: small;
}

h1 {
    font-size: 200%;
    font-family: "Emblema One";
}

h2 {
    font-size: 140%;
    font-weight: normal;
}

h1, h2 {
    color: #0cb8cc;
    text-decoration: line-through;
}

blockquote, q {
    font-style: italic;
}
```








































# 盒模型

CSS使用一个盒模型来控制元素如何显示。

盒子由：

- 内容区(content)
内容区包含元素的内容。

- 可选的 内边距(padding)、边框(border)、外边距(margin)
内边距用来在内容区周围创建可见的空间。
边框包围内边距和内容，它提供了从视觉上分离内容的一种手段。
外边距包围边框、内边距和内容，允许在元素和其它元素之间增加空间。

内边距和外边距都可以使用像素或百分数设置。
边框宽度可以用像素设置，也可以使用关键字(thin, mediun, thick)来指定。
对于外边距、内边距或边框，CSS提供了相应的属性，可以一次对所有四个边(上下左右)完成设置，也可以单独设置任意一边。

元素的背景会在内容和内边距下显示，但不会延伸到外边距下面。
有八种不同的边框样式，包括实现、破折线、虚线和脊线。

对于希望成组指定样式的元素要使用`class`属性。
使用`id`属性为元素指定一个唯一的名字，还可使用`id`属性为元素提供唯一的样式。
可以使用`id`选择器按id选择元素。
一个元素只能有一个`id`，不过它可以属于多个类`class`。

在HTML中可以使用多个样式表。
如果两个样式表包含冲突的属性定义，HTML文件中最后链接的样式表最为优先。
可以在`<link>`元素中使用媒体查询或使用CSS中的`@media`规则来指定设备。

<br>

**元素**

```
padding-top:
padding-left:
margin-right:
margin-bottom:
border-radius: 对有边框的元素创建圆角
border-top-left-radius：
border-right-bottom-radius：
border-left-style：solid实线 / double双线 / groove槽线 / outset外凸 / inset内凹 / dotted虚线 / dashed破折线 / ridge脊线
border-top-width：thin/ medium/ thick/ px
border-right-color：
background-image：url（images/*.jpg）或 url（http://*http://*）
background-position：top left/ right bottom/ center
background-repeat：no-repeat/ repeat/ repeat-x/ repeat-y/ inherit（按父元素的设置来处理）

```

<br>

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="utf-8">
    <title>9</title>
    <link rel="stylesheet" href="9.css">
</head>

<body>
    <strong><img src="images/9.png" alt="9"></strong>
    <h1>99</h1>

    <p>
        9
        99
        999
    </p>

    <p id="wahaha">
        wa
        haha
    </p>

    <h2>999</h2>
    <p>
        9
        99
        <ul>
            <li>999</li>
            <li>999</li>
            <li>999</li>
        </ul>
    </p>
</body>

</html>
```

```css
body {
    font-family: Verdana, Helvetica, Arial, sans-serif;
    font-size: small;
    line-height: 1.5em;
}

h1, h2 {
    color: rgba(118, 173, 226, 0.86)
}

h1 {
    font-size: 170%;
}

h2 {
    font-size: 1.6em;
}

strong {
    margin-left: 35%;
    margin-right: 20%;
}

#wahaha {
    font-family: Georgia, "Times New Roman", Times, Serif;
    line-height: 1.9em;
    font-style: italic;
    background-color: #ececec;
    background-image: url(images/9.gif);
    background-repeat: no-repeat;
    background-position: left top;
    border-color: white;
    border-radius: 20px;
    border-width: 2px;
    border-style: dashed;
    padding: 25px;
    padding-left: 60px;
    margin: 30px;
    margin-right: 250px;
}
```




















<br/>
<br/>

---

<br/>
<br/>
























# div和span

`<div>`元素用于将相关的元素归组在一起，放在逻辑区中。
创建逻辑区有助于标识主要内容区，以及页面的页眉和页脚。

可以使用`<div>`元素将需要共同样式的元素归组在一起。
使用嵌套`<div>`元素为文件增加更多结构，这有利于保证结构清晰或方便增加样式。不过除非确实需要，否则不要过多地增加结构。
一旦使用`<div>`将内容区归组在一起，类似于其它块元素，可以对这些`<div>`增加样式。例如，对于包含在`<div>`中的一组元素，可以使用嵌入这些元素的`<div>`的边框属性，对这组元素增加一个边框。

`width`属性设置设置一个元素内容区(content)的宽度。
一个元素的总宽度是内容区宽度，加上所增加的页边距、边框和外边距的宽度。
一旦设置一个元素的宽度，它不会延伸来占满浏览器窗口的整个宽度。

`text-align`是块元素的一个属性，用来将这个块元素中的所有内容对其，可以居中、左、右对齐。这个属性可以由所有嵌套的块元素继承。

可以使用子孙选择器来选择嵌套在其它元素中的元素。如子孙选择器`div h2 {...}`。会选择嵌套在`<div>`元素中的所有`<h2>`(包括子元素、孙子元素等)。

可对相关的属性使用快捷方式。如`padding-top, padding-bottom, padding-left, padding-right`都与内边距有关，可以用一个快捷规则来指定: `padding`。

`<span>`内联元素与`<div>`元素类似，它用于将相关的内联元素(inline)和文本归组在一起。
类似于`<div>`，可以将`<span>`元素增加到类（或者为`<span>`元素指定唯一的id），对它们增加样式。

有些元素有不同的状态，`<a>`元素就是这样一个例子。`<a>`元素的主要状态包括 未访问、已访问、悬停。

可以用 伪类(pseudo class) 单独地为各个状态指定样式。伪类最常用于`<a>`元素。`:link`对应未访问的链接；`:visited`对应已访问链接；`:hover`对应悬停状态。
伪类还可以用于其它元素，而不仅限于`<a>`元素。
另外一些伪类包括：`:acrivate`, `:focus`, `:first-child`, `:last-child`等。

<br>

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>10</title>
    <link rel="stylesheet" href="10.css">
</head>
<body>
    <strong><img src="images/10.png" alt="10"></strong><br>

    <div id="elixirs">
    <h2>1010</h2><br>
    <img src="images/1010.png" alt="1010">
    <p>
        10
        1010
    </p>
    </div>

    <p id="guarantee">
        10
        1010
        101010
    </p>
    <p class="pseudo">
        10
        1010
    </p>

    <h2>101010</h2>
    <p>
        10
        1010
        101010
        <ul>
            <li>
                <span class="cd">aaa</span>,
                <span class="artist">bbb</span>
            </li>
            <li>
                <span class="cd">AAA</span>,
                <span class="artist">BBB</span>
            </li>
        </ul>
    </p>

    <div id="copyright">
        <p>
            @xxxx<br>
            xxxx
        </p>
    </div>
</body>
</html>
```

```css
body {
    font-family: Verdana, Helvetica, Arial, sans-serif;
    font-size: small;
    line-height: 1.5em;
}

h1, h2 {
    color: rgba(118, 173, 226, 0.86);
}

h1 {
    font-size: 170%;
}

h2 {
    font-size: 1.6em;
}

strong {
    margin-left: 35%;
    margin-right: 20%;
}

#guarantee {
    font: italic small/1.9em Feorgia, "Times New Roman", Times, Serif;
    background: #ececec url(images/bg.gif) no-repeat top left;
    border: white 2px dashed;
    border-radius: 20px;
    padding: 25px;
    padding-left: 60px;
    margin: 30px;
    margin-right: 250px;
}

#elixirs {
    border: thin solid #007e7e;
    width: 200px;
    /* 顺时针 top right bottom left */
    padding: 0 20px 20px 20px;
    margin-left: 20px;
    text-align: center;
    background: url(images/bg2.gif) repeat-x top right;
    float: right;
}

#elixirs h2 {
    color: black;
}

#elixirs p {
    line-height: 1.5em;
}

#copyright {
    font-size: 50%;
    text-aligh: center;
    line-height: normal;
    margin-top: 30px;
}

.cd {
    font-style: italic;
}

.artist {
    font-weight: bold;
}

#elixirs a:link {
    color: #0cb8cc;
}
#elixirs a:visited {
    color: red;
}
#elixirs a:hover {
    background: #16a085;
    text-decoration: line-through;
}

p.pseudo a:hover {
    background: #333333;
}
```


























<br/>
<br/>

---

<br/>
<br/>
























# 布局与定位

浏览器使用流(flow)在页面中放置元素。

块元素从上向下流，各元素之间有一个换行。默认的，每个块元素会占浏览器窗口的整个宽度。
内联元素在块元素内部从左上方流向右下方。如果需要多行，浏览器会换行，在垂直方向上扩展外围块元素，来包含这些内联元素。

正常页面流中两个块元素上下相邻的外边距会折叠为最大外边距的大小，或者如果两个外边距大小相同，则会折叠为一个外边距。
浮动元素会从正常流中取出，浮动到左边或者右边。
浮动元素放在块元素之上，不会影响正常的页面流。不过，内联内容会考虑浮动元素的边界，围绕这个浮动元素。

`clear`属性用来指定一个块元素左边或右边（或左右两边）不能有浮动元素。设置了`clear`属性的块元素会下移，直到它旁边没有块元素。

浮动元素必须有特定的宽度，不能设置为`auto`。

流体布局是指，扩展浏览器窗口时，页面中的内容会扩展以适应页面。
冻结布局是指，其中内容的宽度是固定的，不会随着浏览器窗口扩展或收缩。这有一个好处，可以对设计提供更多控制，不过也要付出代价，这样就不能有效地使用浏览器宽度了。
凝胶布局是指，其中内容的宽度时固定的，但是外边距会随着浏览器窗口扩展或收缩。凝胶布局通常会把内容放在中央。这与冻结布局有同样的好处，不过通常更美观。

`positon`属性可以设置四个值：static(静态)、absolute(绝对)、fixed(固定)、relative(相对)。
静态定位是默认方式，将元素放在页面的正常流中。
绝对、固定和相对定位时，属性`top, right, bottom, left`可用来指定元素的位置。
绝对定位元素可以使用`z-index`属性分层放置，是一个元素在另一个元素上面。`z-index`值越大，说明它的层次越高（在屏幕上离你越近）。
固定定位元素总是相对于浏览器窗口定位，页面滚动时，固定定位的元素不会移动。页面中的其它内容会在这些固定定位元素下面正常滚动。
相对定位元素首先正常流入页面，然后按指定的量偏移，从而留出它们原先所在的空间。
使用相对定位时，`left, right, top, bottom`是指距正常流中该元素位置的偏移量。

CSS表格显示允许按一种表格布局来摆放元素。
要创建CSS表格显示，需要使用对应表格的一个块元素，对应行的块元素。以及对应单元格的块元素。通常，这些块元素都是`<div>`元素。

如果需要建立多栏布局，而且内容栏是均匀的，表格显示就是一个很好的布局策略。

<br>

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>11</title>
    <link ref="stylesheet" href="11.css">
</head>
</body>
    <div id="header">
        <div id="header1">
            <img src="images/11.png" alt="11">
        </div>
        <div id="header2">
            <img src="images/bg2.gif">
        </div>
        <div id="header3">
            <img src="images/bg3" width="200px" height="200px">
        </div>
    </div>

    <div id="tableContainer">
        <div id="tableRow">
            <div id="drinks">
                <h1>11</h1>
                <p>1111</p>
            </div>
            <div id="main">
                <h1>11</h1>
                <p>
                    11
                    1111
                </p>
                <p>
                    1111
                    111111
                </p>
                <p class="pseudo">
                    11 11
                    1111 1111
                </p>
                <p>
                    11 11 11
                    <ul>
                        <li>
                            <span class="cd">aaa</span>,
                            <span class="artist">bbb</span>
                        </li>
                        <li>
                            <span class="cd">AAA</span>,
                            <span class="artist">BBB</span>
                        </li>
                    </ul>
                </p>
            </div>
            <div id="sidebar">
                <h2>11 11 11</h2>
                <img src="iamges/img2.ico" alt="img2">
                <p>
                    11
                    11 11
                    11 11 11
                </p>
            </div>
        </div>
    </div>

    <div id="copyright">
        <p>
            @2016,xxxx<br>
            xxxx
        </p>
    </div>
</body>
</html>
```

```css
body {
    font-family: Verdana, Helvetica, Arial, sans-serif;
    font-size: small;
    line-height: 1.5em;
}

h1, h2 {
    color: rgba(118, 173, 226, 0.86);
}

h1 {
    font-size: 170%;
}

h2 {
    font-size: 1.6em;
}

#header1 {
    margin-left: 35%;
    margin-right: 40%;
}

#header2 {
    position: fixed;
    top: 300px;
    left: 0px;
}

#header3 {
    position: absolute;
    top: 0px;
    left: 0px;
}

#sidebar {
    display: table-cell;
    font-size: 105%;
    padding: 15px;
    vertical-align: top;
}

#sidebar h2 {
    color: black;
}

#sidebar h3 {
    color: #d12c47;
}

#sidebar p {
    line-height: 1.5em;
}

#sidebar a:link {
    color: #0cb8cc;
}
#sidebar a:visited {
    color: red;
}
#sidebar a:hover {
    background: #16a085;
    text-decoration: line-through;
}

#copyright {
    font-size: 50%;
    color: #b9311b;
    font-weight: bold;
    text-align: center;
    line-height: normal;
    clear: both;
    margin: 0px 10px 10px 10px;
}

.cd {
    font-style: italic;
}

.artist {
    font-weight: bold;
}

a {
    text-decoration: none;
}

#main {
    display: table-cell;
    font-size: 105%;
    padding: 15px;
    vertical-align: top;
}

#tableContainer {
    display: table;
    border-spacing: 10px;
}

#tableRow {
    display: table-row;
}

#drinks {
    display: taable-cell;
    width: 15%;
    vertical-aligh: top;
    padding: 15px;
}
```


























<br/>
<br/>

---

<br/>
<br/>





























# HTML5

```
<aside>    # 用于表示放在内容旁边的内容，比如引用
<mark>     # 用于突出显示某些文件，就像记号笔一样棒
<audio>    # 用于在页面中包含声音内容
<time>     # 时间、日期或日期时间
<progress> # 需要显示任务的完成进度，如90%
<footer>   # 定义一个区块或整个文档的页眉
<header>   # 有首部的区块和整个文档的页眉可以使用这个元素
<meter>    # 需要显示某个范围的度量。比如一个0到220度的温度计，现在显示90度
<article>  # 用来标记类似新闻报道或博客帖子等独立内容
<canvas>   # 在页面中显示js绘制的图像和动画
<section>  # 定义文档的主要区块
<nav>      # 把网站中用于导航的所有链接组织在一起
<figure>   # 定义类似照片、图像甚至代码清单等独立的内容
<video>    # 在页面中加入一个视频
```

视频编码是用来创建视频文件的编码。
视频容器文件包含视频、音频和元数据。
要提供多个视频源文件，确保你的用户可以在他们的浏览器中播放视频文件。

<br>

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset='utf-8'>
    <title>12</title>
    <link rel="stylesheet" href="12.css">
</head>
<body>
    <header class="top">
        <img src="images/12.png" alt="12">
    </header>
    <nav>
        <ul>
            <li><a href="1.html"></a></li>
            <li><a href="2.html"></a></li>
            <li><a href="3.html"></a></li>
        <ul>
    </nav>
    <div id="tableContainer">
        <div id="tableRow">
            <section id="drinks">
              <p>x</p>
              <p>xx</p>
            </section>
            <section id="blog">
                <article>
                    <header>
                        <h1>blog</h1>
                        <time datetime="2019-01-01">Jan 01 2019</time>
                    </header>
                    <p>
                        12
                        12 12
                        12 12 12
                    </p>
                </article>
            <section>
            <aside>
                <h2>12 12</h2><br>
                <img src="images/1212.png" alt="1212">
                <p>
                    12 12
                    12
                    12 12
                </p>
            </aside>
        </div>
    </div>
    <footer>
        <p>
            @2019,xxx
            xxxx
        </p>
    </footer>
</body>
</html>
```






















<br/>
<br/>

---


<br/>
<br/>


































# 表格与更多列表

确实需要在页面中创建表格数据时，就使用HTML表格。
如果只需要对其它类型的内容使用一种类似与表格的表现方式，就可以使用CSS表格来显示布局。

HTML表格用来建立表格数据结构。
HTML表的元素： 表格(`<table>`)、表行(`<tr>`)、表头(`<th>`)、表格数据(`td`)，一齐用来创建一个表格。
`table`元素定义并包围整个表格；
表格使用`<tr>`元素按行定义；
每行包含一个或多个数据单元格，用`<td>`元素定义；
表格采用格状布局。每行对应HTML中的一个`<tr>..</tr>`，每列对应行中的`<td>...</td>`内容。
如果一个数据单元格没有数据，`<td>`元素中不放置任何内容。不过，需要使用`<td></td>`维持表格对齐。
如果你的数据单元格需跨多行或多列，可使用`<td>`元素的`rowspan`或`colspan`属性。

可以用`<caption>`元素提供关于表格的额外信息。

表格有边框间距，也就是单元格之间的间距。
表格数据单元格还可以有内边距和边框，但没有外边距。
就像能够控制元素的内边距、边框、外边距一样，可以用CSS控制表格单元格的内边距、边框和边框间距。

`border-collapse`(边框折叠)是针对表格的一个特殊CSS属性，允许将单元格边框合并为一个边框，让外观更简洁。

可以用`text-align`和`vertic-align` CSS属性改变表格单元格中数据的对齐方式。
可以用`background`属性为表格增色。可以为整个表格、各行或单个的数据单元格增加背景色。

使用CSS的`:nth-child`伪类可为表格隔行增加背景色。

可以在表格中嵌套表格，将`<table>`元素及其内容放在一个数据单元格中。

表格应当应用于表示表格数据，而不是建立页面布局。

<br>

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>13</title>
    <link rel="syslesheet" href="../css/13.css">
</head>
<body>
    <h1>13</html>
    <p>13 13 13 13</p>
    <h2>2019-01-10</h2>
    <img src="../images/13.png">
    <p>
      13
      13 13
      <table>
      <cpation><b>13 13</b></caption>
      <tr>
          <th>L1</th>
          <th>L2</th>
          <th>L3</th>
      </tr>
      <tr>
          <td>1</td>
          <td>2</td>
          <td>3</td>
      </tr>
      <tr>
          <td>11</td>
          <td>22</td>
          <td>33</td>
      </tr>
      <tr class="cellcolor">
          <td>1c</td>
          <td>2c</td>
          <td>3c</td>
      </tr>
      <tr>
          <td rowspan="2">111</td>
          <td colspan="2">222</td>
          <td rowspan="2">333</td>
      </tr>
      <tr>
          <td>1111</td>
          <td>2222</td>
          <td>
              <table>
                  <tr>
                      <td>1 1 </td>
                      <td>2 2 </td>
                  </tr>
                  <tr>
                      <td>1 1 1</td>
                      <td>2 2 2</td>
                  </tr>
              </table>
          </td>
      </tr>
      </table>
    </p>
</body>
</html>
```















<br/>
<br/>

---

<br/>
<br/>





















# 表单

`<form>`元素定义了表单，所有表单输入元素都嵌套着这个元素中。

```
action 属性包含了服务器脚本的URL
method 属性包含发送表单数据的方法，可以是GET或POST

POST 打包数据表单，并把它作为请求的一部分发送到服务器
GET 打包表单数据，并把数据追加到url

如果表单数据应当是私有的，或者表单数据很多，如使用了一个<textarea>或者file <input>元素，就应当使用POST方法。
对于可以加书签的请求，要使用GET方法。

<input>元素在web页面上可以作为多种不同的输入控件，这取决于它的type属性值。
type为 text 时会创建一个单行文本输入框
type为 submit 时会创建一个提交按钮
type为 radio 时会创建一个单选框。所有同名的单选框构成一组互斥的按钮
type为 checkbox 时会创建一个复选框控件。通过为多个复选框指定相同的名字，可以创建一组选择
type为 number 时会创建一个只允许数字字符的单行文本输入控件
type为 range 时会创建一个滑动条控件提供数字输入

color 类型会在支持这个类型的浏览器中创建一个颜色选择器（否则只会创建一个普通的文本输入控件）
date 类型会在支持这个类型的浏览器中创建一个日期选择器（否则只会创建一个普通的文本输入控件）
email, url, tel 类型会创建当行文本输入，在一些移动浏览器上出现定制键盘来方便数据输入

<textarea> 元素会创建一个多行文本输入区。
<select> 元素会创建一个菜单，包含一个或多个 <option> 元素。<option> 元素定义了菜单中的菜单项。

如果将文本放在 <textarea> 元素的内容中，这会成为web页面上文本去控件中的默认文本

text <input> 元素中的 value 属性可以用来为单行文本输入控件提供一个初始值。
在提交按钮设置 value 属性可以改变按钮上显示的文本。

提交一个web表单时，表单数据值与相应的数据名配对，所有名和值会发送到服务器。

由于表单有一个表格结构，通常会用CSS表格显示来建立表单布局。CSS还可以用来指定表单的颜色、字体风格、边框样式。

HTML允许用 <fieldset> 元素组织表单元素。

可以用 <lable> 元素以一种有助于提高可访问性的方式关联标签与表单元素。

使用 placeholder 属性可以为表单用户提供一个提示， 指出你希望在一个输入域中输入什么内容。

required 属性指示一个输入域是必要的，要让表单成功提交，这个输入域中必须有值。有些浏览器在你提交表单之前会强制要求在这些域中输入数据。
```

<br>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Form</title>
    <link ref="stylesheet" href="form.css">
</head>
<body>
    <h1>Form 1</h1>
    <h2>Form 2</h2>
    <form action="http://xxx.com/xx.php" method="post">
        <div class="tableRow">
            <p> Choose: </p>
            <p>
                <select name="beans">
                    <option value="default">111</option>
                    <option value="default">222</option>
                    <option value="default">333</option>
                    <option value="default">444</option>
                </select>
            </p>
        </div>
        <div class="tableRow">
            <p>Type: </p>
            <p>
                <input type="radio" name="beantype" value="whole">55<br>
                <input type="radio" name="beantype" value="ground">66
            </p>
        </div>
        <div class="tableRow">
            <p>Number:</p>
            <p>
                <input type="number" name="bags" min="1" max="20" required>
            </p>
        </div>
        <div class="tableRow">
            <p>Date:</p>
            <p>
                <input type="date" name="date">
            </p>
        </div>
        <div class="tableRow">
            <p>Name: </p>
            <p>
                <input type="text" name="name" value="">
            </p>
        </div>
        <div class="tableRow">
            <p>Address: </p>
            <p>
                <input type="text" name="address"value="" required>
            </p>
        <div class="tableRow">
            <p>Phone: </p>
            <p>
                <input type="tel" name="phone" value="" required>
            </p>
        </div>
        <div class="tableRow">
            <p>Email: </p>
            <p>
                <input type="email" name="email" value="" placeholder="someone@example.com">
            </p>
        </div>
        <div class="tableRow">
            <p>Choose file:</p>
            <p>
                <input type="file" name="file">
            </p>
        </div>
        <div class="tableRow">
            <p>Comment: </p>
            <p>
                <textarea name="comment"></textarea>
            </p>
        </div>
        <div class="tableRow">
            <p>
                <input type="submit" value="Order">
            </p>
        </div>
    </form>
</body>
</html>
```

```css
body {
    background: #efe5d0;
    margin: 20px;
    font-family: Georgia, "Times New Roman", Times, Serif;
}

form {
    display: table;
    padding: 10px;
    border: thin dotted #7e7e7e;
    background: #e1ceb8;
    margin: 10px 200px;
}

form textarea {
    width: 500px;
    height: 150px;
}

div.tableRow {
    display: table-row;
}

div.tableRow p {
    display: table-cell;
    vertical-align: top;
    padding: 3px;
}

div.tableRow p:first-child {
    text-align: right;
}

p.heading {
    font-weight: bold;
}

h1, h2 {
    text-align: center;
}
```
