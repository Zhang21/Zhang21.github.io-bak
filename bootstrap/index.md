# Bootstrap



参考：

- Bootstrap中文文档: <https://v3.bootcss.com/>

环境：

- CentOS7x86_64
- Bootstrap v3











<br/>
<br/>

---

<!--more-->

<br/>
<br/>



















# 概述

Bootstrap是最受欢迎的HTML、CSS和JS框架，用于开发响应式布局、移动设备优先的WEB项目。
Bootstrap让前端开发更快速、简单。所有开发者都能快速上手、所有设备都可以适配、所有项目都适用。




<br/>
<br/>

---

<br/>
<br/>


























# 起步


## 安装



<br/>
<br/>
<br/>




## 包含的内容


Bootstrap 提供了两种形式的压缩包，在下载下来的压缩包内可以看到以下目录和文件，这些文件按照类别放到了不同的目录内，并且提供了压缩与未压缩两种版本。

> Bootstrap 插件全部依赖 jQuery
> 请注意，Bootstrap 的所有 JavaScript 插件都依赖 jQuery，因此 jQuery 必须在 Bootstrap 之前引入，就像在基本模版中所展示的一样。

<br>

预编译版，预编译文件可以直接使用到任何 web 项目中。

```
bootstrap/
├── css/
│   ├── bootstrap.css
│   ├── bootstrap.css.map
│   ├── bootstrap.min.css
│   ├── bootstrap.min.css.map
│   ├── bootstrap-theme.css
│   ├── bootstrap-theme.css.map
│   ├── bootstrap-theme.min.css
│   └── bootstrap-theme.min.css.map
├── js/
│   ├── bootstrap.js
│   └── bootstrap.min.js
└── fonts/
    ├── glyphicons-halflings-regular.eot
        ├── glyphicons-halflings-regular.svg
            ├── glyphicons-halflings-regular.ttf
                ├── glyphicons-halflings-regular.woff
                    └── glyphicons-halflings-regular.woff2
```

<br>

Bootstrap源码，源码包含了预先编译的 CSS、JavaScript 和图标字体文件，并且还有 LESS、JavaScript 和文档的源码。

```
bootstrap/
├── less/
├── js/
├── fonts/
├── dist/
│   ├── css/
│   ├── js/
│   └── fonts/
└── docs/
    └── examples/
```



<br/>
<br/>
<br/>



## 编译CSS和JS文件





<br/>
<br/>
<br/>



## 基本模板

这份超级简单的 HTML 模版，我们强烈建议你对这些实例按照自己的需求进行修改，而不要简单的复制、粘贴。

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- Bootstrap -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" rel="stylesheet">

    <!-- HTML5 shim 和 Respond.js 是为了让 IE8 支持 HTML5 元素和媒体查询（media queries）功能 -->
    <!-- 警告：通过 file:// 协议（就是直接将 html 页面拖拽到浏览器中）访问页面时 Respond.js 不起作用 -->
    <!--[if lt IE 9]>
      <script src="https://cdn.jsdelivr.net/npm/html5shiv@3.7.3/dist/html5shiv.min.js"></script>
      <script src="https://cdn.jsdelivr.net/npm/respond.js@1.4.2/dest/respond.min.js"></script>
    <![endif]-->
  </head>
  <body>
    <h1>你好，世界！</h1>

    <!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
    <script src="https://cdn.jsdelivr.net/npm/jquery@1.12.4/dist/jquery.min.js"></script>
    <!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/js/bootstrap.min.js"></script>
  </body>
</html>
```



<br/>
<br/>
<br/>



## 实例精选


我们鼓励你根据自身项目的需要对 Bootstrap 进行定制和修改。

<br>

###  框架基本用法

- 入门级模板(starter template)
只有基本的东西：引入了预编译版的 CSS 和 JavaScript 文件，页面只包含了一个 `container` 元素。

![](/images/Bootstrap/starter-template.jpg)

- Bootstrap主题(theme)
加载可选的 Bootstrap 主题，获得增强的视觉体验。

![](/images/Bootstrap/theme.jpg)

- 栅格(grids)
多个关于栅格布局方面的实例，涉及到层级（tier）、嵌套（nesting）等等。

![](/images/Bootstrap/grid.jpg)

- Jumbotron
Build around the jumbotron with a navbar and some basic grid columns.

![](/images/Bootstrap/jumbotron.jpg)

- Narrow jumbotron
Build a more custom page by narrowing the default container and jumbotron.

![](/images/Bootstrap/jumbotron-narrow.jpg)


<br/>
<br/>


### 导航条实例(navbars)

- 导航条(navbar)
包含导航条和一起附加内容的超级基础的模板。

![](/images/Bootstrap/navbar.jpg)

- 静态导航条(static top navbar)
包含一个静态导航条以及一些附加内容的超级基础的模板。

![](/images/Bootstrap/navbar-static.jpg)

- 固定位置的导航条(fixed navbar)
这是一个超简单的页面，拥有一个固定在顶部的导航条和一些演示内容。

![](/images/Bootstrap/navbar-fixed.jpg)


<br/>
<br/>


### 自定义组件(customed Components)

- 封面图(cover)
一个简单、漂亮的首页。

![](/images/Bootstrap/cover.jpg)

- Carousel
Customize the navbar and carousel, then add some new components.

![](/images/Bootstrap/carousel.jpg)

- 博客页面(blog)
简单的两列式博客布局，还包含了自定义的导航、页头、分类等元素。

![](/images/Bootstrap/blog.jpg)

- 控制台(dashboard)
包含基本结构的后台管理模板，还有固定的侧边栏和导航条。

![](/images/Bootstrap/dashboard.jpg)

- 登录页(sign-in)
自定义的表单布局以及经过简单设计的登录表单。

![](/images/Bootstrap/sign-in.jpg)

- Justified nav
Create a custom navbar with justified links.

![](/images/Bootstrap/justified-nav.jpg)

- Sticky footer
Attach a footer to the bottom of the viewport when the content is shorter than it.

![](/images/Bootstrap/sticky-footer.jpg)

- Sticky footer with navbar
Attach a footer to the bottom of the viewport with a fixed navbar at the top.

![](/images/Bootstrap/sticky-footer-navbar.jpg)


<br/>
<br/>


### 实现性案例(Experiments)

- 非响应式Bootstrap布局(Non-responsive)
Easily disable the responsiveness of Bootstrap per our docs.

![](/images/Bootstrap/non-responsive.jpg)

- Offcanvas
Build a toggleable off-canvas navigation menu for use with Bootstrap.

![](/images/Bootstrap/offcanvas.jpg)



<br/>
<br/>
<br/>




## 工具

Tools



















<br/>
<br/>

---

<br/>
<br/>






















# CSS

docs: https://getbootstrap.com/docs/3.3/css/
docs-cn: https://v3.bootcss.com/css/


<br/>


## 概览

设置全局CSS样式；基本的HTML元素均可以通过class设置样式并得到增强效果；还有先进的栅格系统。

深入了解 Bootstrap 底层结构的关键部分，包括我们让 web 开发变得更好、更快、更强壮的最佳实践。


<br/>


### HTML5文档类型

HTML5 DOCTYPE

Bootstrap使用到某些HTML元素和CSS属性需要将页面设置为HTML5文档类型。在你项目的每个页面下都要参照下面的格式进行设置。

```html
<!DOCTYPE html>
<html lang="zh-CN">
  ...
</html>
```


<br/>
<br/>


### 移动设备优先

Bootstrap 是移动设备优先的。


<br/>
<br/>


### 排版与链接



