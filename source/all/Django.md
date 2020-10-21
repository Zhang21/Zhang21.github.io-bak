---
title: Django
date: 2019-09-25 10:59:21
mathjax: true
categories: DevOps
tags:
  - Django
  - Python
---



参考:
- docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/>

<br>

版本:
- Django Version: v2.1





<br/>
<br/>

---

<!--more-->

<br/>
<br/>









# 使用Django

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/>




<br/>
<br/>




# 安装Django









<br/>
<br/>

---

<br/>
<br/>











# 模型和数据库

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/db/>


模型是你的数据唯一而且准确的信息来源。它包含你正在存储的数据的重要字段和行为。一般来说，每一个模型都映射一个数据库表。


<br/>


## 模型

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/db/models/>

基础:

- 每个模型都是一个Python类(继承`django.db.models.Model`)
- 模型类的每个属性都相当于一个数据库的字段

<br/>


### 快速上手

这个样例模型定义了一个Person, 其拥有`first_name`和`last_name`:

```py
from django import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
```

`first_name`和`last_name`是模型的字段。每个字段都被指定为一个类属性，并且每个属性映射为一个数据库列。

上面的Person会创建一个如下的数据库表:

```sql
CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);
```

说明:

- 表名`myapp_person`是自动从模型元数据中派生出来，但可以被改写
- `id`字段会被自动添加，也可被改写
- 这是使用默认的PostgreSQL语法，数据库引擎可在settings中配置


<br/>
<br/>


### 使用模型

一旦定义了模型，需要告诉Django使用这些模型。需要修改`setting`文件中的`INSTALLED_APPS`, 这个设置中添加包含你`models.py`文件的模块的名字。

```
# 例如，模型位于项目中的myapp.models中
# 包结构使用 manage.py startapp 创建

INSTALLED_APPS = [
  ...
  'myapp',
  ...
]
```

当你在配置中添加新应用的时候，请务必运行`manage.py migrate <migrate>`，此外你也可以先使用`manage.py makemigrations`先进行迁移。




<br/>
<br/>
<br/>




## 进行查询

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/db/queries/>


一旦创建了数据模型，Django会自动地给你一个数据库抽象API——让你可创建、检索、更新和删除对象。

```py
from django.db import models

class Blog(models.Model):
    name = models.CharField(max_length=1000)
    tagline = models.TextField()

    def __str__(self):
        return self.name

class Author(models.Model):
    name = models.CharField(max_length=1200)
    email = models.EmailField()

    def __str__(self):
        return self.name


class Entry(models.Model):
    blog = models.ForeignKey(Blog, on_delete=models.CASCADE)
    headline = models.CharField(max_length=255)
    body_text = models.TextField()
    pub_date = models.DateField()
    authors = models.MangToManyField(Author)
    n_comments = models.IntegerField()
    n_pingbacks = models.IntegerField()


    def __str__(self):
        return self.headline
```


<br/>


### 创建对象

为了表示在Python对象数据库表中的数据，Django使用一个直观的系统:

- 一个模型类表示一个数据库表
- 类实例表示数据库中的表中的特定记录

要创建一个对象，使用关键字参数模型类的实例化，然后调用`save()`将它保存到数据库中。

```py
# 例子
from blog.models import Blog

b = Blog(name='xxx', tagline='xxxx')
b.save()
```


<br/>
<br/>


### 将更改保存到对象

使用`save()`将更改保存到一个对象中已有的数据库。

```py
b5.name = 'New b5'
b5.save()

# 这执行幕后的UPDATE SQL语句
```


<br/>
<br/>


### 检索对象

要检索数据库对象，通过在模型类上`Manager`构建一个`QuerySet`。
`QuerySet`代表从数据库的对象集合。它可以有0, 1, n个过滤器。在SQL方面，一个`QuerySet`相当于`SELECT`语句，而过滤器相当于`WHERE, LIMIT`。
通过使用`Manager`模型获取`QuerySet`。每个模型至少有一个`Manager`，这就是所谓的默认`objects`。直接通过模型类访问:

```py
Blog.objects

b = Blog(name='xxx', tagline='Bar')
b.objects
...
```

检索所有对象；

使用过滤器检索特定对象；


<br/>
<br/>


### 比较对象



<br/>
<br/>


### 删除对象


<br/>
<br/>


### 更新对象



<br/>
<br/>


### 关联对象


<br/>
<br/>


### 回落的原生SQL

如果你发现自己使用Django 数据库映射器写一个SQL查询太复杂，你可以手边编写SQL fall back。




<br/>
<br/>
<br/>




## 聚合

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/db/aggregation/>

有时候要获取的值需要根据一组对象聚合后才能得到。




<br/>
<br/>



## 搜索

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/db/search/>

Web应用的一个常见任务就是寻找与用户输入数据库中的某些数据。











<br/>
<br/>

---

<br/>
<br/>



# 处理HTTP请求

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/http/>


<br/>


## URL调度器

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/http/urls/>

对于高质量的Web应用来说，使用简洁、优雅的URL模式是一个非常值得重视的世界。Django允许你自由滴设计你的URL，不受框架束缚。


<br/>


### 概况

为了给一个应用设计URL，你需要创建一个Python模块，通常被称为`URLconf`。这个模块是纯粹的Python代码，包含URL模式(简单的正则)到Python函数(视图)的简单映射。
映射可长可短，随便你。它可以引用其它映射。而且，因为它是纯粹的Python代码，它可以动态构造。


<br/>
<br/>


### Django如何处理一个请求

当一个用户请求Django站点的一个页面，下面是Django系统决定执行哪个Python代码使用的算法:

1. Django确定 root URLconf 模块的使用。通常，这个`setting`中`ROOT_URLCONF`的值，但如果传入HttpRequest对象具有URL配置属性(由中间件设置)，它的值将代替`ROOT_URLCONF`。
2. Django载入 Python 模块并查找 `urlpatterns`变量。这应该是是一个 Python List。
3. Django依次匹配每个URL模式，在与请求的URL匹配的第一个模式停下来。
4. 一旦URL模式匹配，Django导入并调用给定的视图——这是一个简单的Python函数(或基于类的视图)。该类会传递如下参数:
  4.1 一个HttpRequest实例
  4.2 如果匹配的URL模式没有返回命名组，则从正则匹配作为位置参数
  4.3 关键字参数是由通过路径表达式匹配的任何命名的部分，通过在任选关键字参数`kwargs`指定的任何参数重写`django.urls.path()`或`django.urls.re_path()`
5. 如果每个URL模式匹配，或者一个异常在此过程中的任何点被抛出，则Django调用适当的错误处理视图。


<br/>
<br/>


### 栗子

一个简单的URLconf:

```py
from django.urls import path
from . import views

utlpatterns = [
    path('articles/2003/', views.special_case_2003),
    path('articles/<int:year>/', views.year_archive),
    path('articles/<int:year>/<int:month>/', views.month_archive),
    path('articles/<int:year>/<int:month>/<slug:slug>/', view.article_detail)
]
```

注意:

- 为了捕获从URL中的值，使用尖括号`<>`
- 捕获的值可任选地包括一个转换器类型。如，使用`<int: name>`来捕获整数参数。如果不包含一个转换器，则为任意string。排除`/`字符，被匹配。
- 没有必要添加开始的斜线`/`，因为每个URL都有。如`articles`而不是`/articles`。

一些请求栗子:

- 一个请求`/articles/2005/03/`将匹配李彪中的第三项。Django将调用函数`view.mouth_archive(request, year=2005, month=3)`。
- `/articles/2003/`将匹配第一个模式，而不是第二个。Django将调用函数`views.special_case_2003(request)`
- `/articles/2003`将不会匹配这些模式，因为每个模式都要求URL以斜线`/`结束
- `/articles/2003/building-a-django-site/`讲匹配最后一个模式。Django将调用函数`view.article_detail(request, year=2003, month=3, slug="building-a-django-site")`


<br/>
<br/>


### path转换器

下面的路径转换器默认可用:

- `str`: 匹配任意非空字符串，不包括路径分隔符`/`。如果表达式中不包含转换器，这是默认值。
- `int`: 匹配零个或多个正整数，返回一个int。
- `slug`: 匹配任何由ASCII字母、数字、连字符、下划线组成的slug字符串。
- `uuid`: 匹配一个格式化的UUID。为了防止映射到同一页面的多个网址，必须包含字符，必须小写。(如`075194d3-6885-417e-a8a8-6c931e272f00`)，返回一个UUID实例。
- `path`: 匹配任意费控字符串，包含路径分隔符`/`。这允许你来匹配一个完整的URL路径.


<br/>
<br/>


### 注册自定义路径转换器

对于更复杂的匹配要求，可以定义自己的路径转换器。



<br/>
<br/>


### 使用正则表达式

如果路径和转换器的语法不够定义你的URL模式，你也可以使用正则表达式。要做到这一点，使用`re_path()`来代替`path()`。

在Python的正则表达式中，正则表达式组名语法是`(?P<name>pattern)`，name是组名和pattern相匹配。

将之前的URLconf使用正则表达式改写:

```py
from django.urls import path, re_path
from . import views

urlpatterns = [
    path('articles/2003/', views.special_case_2003),
    re_path(r'^articles/(?P<year>[0-9]{4}/$', views.year_archive)),
    re_path(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/$', views.month_archive),
    re_path(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<slug>[\w-]+)/$', views.article_detail),
]
```

<br>

使用未命名的正则表达式组

<br>

嵌套的参数

```py
# 正则表达式允许嵌套参数
from django.urls import re_path

urlpatterns = [
    re_path(r'^blog/(page-(\d+)/)?$', blog_articles),                  # bad
    re_path(r'^comments/(?:page-(?P<page_number>\d+)/)?$', comments),  # good
]
```


<br/>
<br/>


### URLconf在什么上查找

请求的URL被看成是一个普通的Python字符串，URLconf在其上查找并匹配。进行匹配时将不包括GET或POST请求方式的参数以及域名。
例如，`https://www.example.com/myapp/`请求中，URLconf将查找`myapp/`。在`https://www.example.com/myapp/?page=3`请求中，URLconf仍将查找`myapp/`。
URLconf不检查使用了哪种请求方法。换句话讲，所有的请求方法——无论是POST, GET, HEAD...，都将路由到相同的函数。


<br/>
<br/>


### 指定视图参数的默认值

有一个方便的小技巧是指定视图参数的默认值。

```py
# URLconf

from django.urls import path
from . import views

utlpatterns = [
    path('blog/', views.page),
    path('blog/page<int:num>', views.page),
]



# View
def page(request, num=1):
    ...

```


<br/>
<br/>


### 错误处理

当Django找不到请求的URL匹配，或将引发异常，Django调用错误处理视图。
这些情况发生时使用的视图通过4个变量指定。它们的默认值应该能满足大部分项目，但也可给他们赋值以进一步自定义。

```
handler400: django.conf.urls.handler400

handler403: django.conf.urls.handler403

handler404: django.conf.urls.handler404

handler500: django.conf.urls.handler500
```


<br/>
<br/>


### 包含其它的URLconfs

在任何时候，你的`urlpatterns`都可以`include`其它URLconf模块。这实际上将一部分URL放置于其它URL下面。

```py
from django.urls import include, path

urlpatterns = [
    ...
    path('community/', include('abc.urls')),
]
```

当Django遇到`include()`，它砍掉任何匹配到该点和该URL的一部分发送所述剩余的字符串所包含的URL配置用于进一步的处理。

```py
from django.urls import include, path

from app.main import view as main_views
from credit import views as credit_views

extra_patterns = [
    path('reports/', credit_views.report),
    path('reports/<int:id>/', credit_views.report),
    path('charge/', credit_views.charge),
]

urlpatterns = [
    path('', main_views.homepage),
    path('help/', include('apps.help.urls')),
    path('credit/', include(extra_patterns)),
]
```

这个栗子中，`/credit/reports/` URL将被`credit_views.report()`这个Django视图处理。

这种方法可以用来去除URLconf中的冗余，其中某个模式前缀被重复使用。如:

```py
from django.urls import path
from . import views

urlpatterns = [
    path('<page_slug>-<page_id>/history/', views.history),
    path('<page_slug>-<page_id>/edit/', views.edit),
    path('<page_slug>-<page_id>/discuss/', views.discuss),
    path('<page_slug>-<page_id>/permissions/', views.permissions),
]



# 改进它
urlpatterns = [
    path('<page_slug>-<page_id>/', include([
        path('history/', views.history),
        path('edit/', views.edit),
        path('discuss/', views.disscuss),
        path('permissions/', views.permissions),
    ])),
]
```

<br>

**捕获的参数**

被包含的URLconf会收到来自父URLconf捕获的任何参数:

```py
# settings/urls/main.py
from django.urls import include, path

urlpatterns = [
    path('<username>/blog/', include('foo.urls.blog')),
]


# foo/urls/blog.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.blog.index),
    path('archive/', views.blog.archive),
]
```

在上面的栗子中，捕获的`username`变量将被如期传递给`include()`执行的URLconf。


<br/>
<br/>


### 传递额外选项到视图函数

URLconfs有一个hooks，可以传递额外的参数给视图函数，作为Python字典。

The `path()` function can take an optional third argument which should be a dictionary of extra keyword arguments to pass to the view function.

```py
from django.urls import path
from . import views

urlpatterns = [
    path('blog/<int:year>/', views.year_archive, {'foo': 'bar'}),
]

# In this example, for a request to /blog/2005/, Django will call views.year_archive(request, year=2005, foo='bar').
```

<br>


**传递额外选项到`include()`**

```py
# main.py
from django.urls import include, path

urlpatterns = [
    path('blog/', include('inner'), {'blog_id': 3}),
]

# inner.py
from django.urls import path
from mysite import views

urlpatterns = [
    path('archive/', views.archive),
    path('about/', views.about),
]
```


<br/>
<br/>



### URLs的反向解析

Django项目工作时的一个常见需要是获得 URLs 它们的最终形式或用于在生成的内容中嵌入的 URL 的可能性为在服务器上的导航流处理测(重定向)。

强烈希望避免硬编码(hard-coding)这些URLs（费力，不可扩展且容易出错）。同样危险的是`ad-hoc`机制，以产生平行于所述 URLconf 描述的设计，这可能导致生成的 URLs 随着时间的推移而变得陈旧。

换句话说，需要一个 DRY 机制。在其它优点将允许 URL 设计的进化，而不必去复习所有项目的源代码来查找和替换过时的URLs。

Django提供了一个解决方案，以使 URL 映射器 是 URL 设计的唯一仓库。用你的 URL 喂养它，那么它可以在两个方向上使用:

- 与 user/browser 请求的 URL 开始，它调用正确的Django视图提供任意参数给需要从URL提取它的值。
- 与标识对应的Django视图将被传递给它的参数的值开始，获得相关的URL。

第一个前面讨论过，第二个是所谓的 URL 的反向解析、URL 反向匹配、 URL 反向查找或 URL 反转。

Django提供了用于执行 URL 反向，匹配不同层 URLs 的需要:

- Template: 使用url template tag
- Python code: 使用 `reverse()` 函数
- 在相关的Django模型实例的URL处理更高级别的代码: `get_absolute_url()`方法

就是给这个path取个名字，通过名字去找路径。即使修改了路径，只要名字没有修改，在其它部分里使用path名字的部分就不需要修改。

```py
from django.urls import path
from . import views

urlpatterns = [
  ...
  path('articles/<int:year>/', views.year_archive, name='news-year-archive'),
]
```

你可通过使用这些模板代码获得:

```jinja2
<a href="{% url 'news-year-archive' 2012 %}">
{# Or with the year in a template context variable: #}
<ul>
{% for yearvar in year_list %}
<li><a href="{% url 'new-year-archive' yearvar %}">{{ yearvar }} Archive</a></li>
{% end for %}
</ul>
```

Or in Python Code:

```py
from django.http import HttpResponseRedirect
from django.urls import reverse

def redirect_to_year(request):
    # ...
    year = 2006
    # ...
    return HttpResponseRedirect(reverse('news-year-archive', args=(year,)))
```


<br/>
<br/>



### 命名URL模式

要执行URL反向，你需要使用命名的URL模式(named URL patterns)。这个名称可以包含任何你喜欢的字符，而不仅限于Python names。
选择不太可能与其它应用程序名称相冲突的名字。如果你调用URL pattern comment 和其它应用程序做同样的事，URL `reverse()`查找取决于哪个模式是最后在项目的 `urlpattens` 的列表中。
为你的URL名称放一个前缀，后去可以从应用程序的名称派生，这样降低了冲突的可能。


<br/>
<br/>


### URL命名空间

URL命名空间允许你唯一地反转 named URL patterns，即使不同的应用程序使用相同的URL名称。

一个URL命名空间有两个部分，两个都是字符串:

- application namespace
- instance namespace


命名空间URL使用特定的`:`操作符。
命名空间同样可以嵌套。命名的URL `sports:polls:index` ，会在顶级命名空间`sports`中定义的`polls`的命名空间中，寻找名为`index`的一个模式。

<br>

**反向命名空间URLs**

当给定一个命名空间URL（如`polls:index`）来解析时，Django将分割名称为几个部分，然后尝试一下查找:

- 首先，Django查找 应用程序命名空间（如`polls`)，这将产生应用程序的实例列表。
- 如果有定义当前应用程序，Django查找并返回该实例的URL解析。
- 如果没有当前应用程序，Django查找默认应用程序实例。
- 如果没有默认的应用程序实例，Django会挑选最后部署的应用的实例。
- 如果提供的命名空间无法匹配步骤1中的应用程序命名空间，Django会尝试将此命名空间直接作为实例命名空间查找。

```py
# urls.py
from django.urls import include, path

urlpatterns = [
    path('author-polls/', include('polls.urls', namespace='author-polls')),
    path('publisher-polls/', include('polls.urls', namespace='publisher-polls')),
]


# polls/urls.py
from django.urls import path
from . import views

app_name = 'polls'
urlpatterns = [
    path('', views.IndexView.as_view(), name='index'),
    path('<int:pk>/', views.DetailView.as_view(), name='detail')
]
```

使用此设置，可能进行下面这些查找:

- 如果其中一个实例就是当前这个，如果我们在实例 `author-polls` 渲染详情页面，`polls:index` 将解析到 `author-polls` 实例页面。即两个都会导致 `/author-polls/`。

在 class-based view 的方法中: `reverse('polls:index', current_app=self.request.resolver_match.namespace)`
在 模板中: ```{% url 'polls:index' %}```

- 如果没有当前实例，xxxx
- `author-polls:index`将总是解析到`author-polls`实例的index页面。

<br>


**URL命名空间和included URLconfs**

included URLconfs的应用程序命名空间可通过两种方式指定：

- 首先，你可在 included URLconf 中设置一个 `app_name` 属性，它与 `urlpatterns` 属性同级。

```py
# polls/urls.py
from django.urls import path
form . import views

app_name = 'polls'
urlpatterns = [
    path('', views.IndexView.as_view(), name='index'),
    ...
]


# urls.py
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls'))
]
```




<br/>
<br/>
<br/>




## 编写视图

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/http/views/>


视图函数，或 view short，是一个简单的Python函数——接收一个Web request，并返回一个Web response。这个response可以是HTML内容的Web页面，重定向，404错误，XML文档或图像...视图本身包含任意的逻辑是必要的返回响应。此代码可以选择你想要的任何地方，只要它是你的Python路径。对于将代码放在何处，默认的约定是把视图放在`views.py`文件中。


<br/>


### 一个简单的视图

一个简单的视图，返回当期的日期和时间:

```py
from django.http import HttpResponse
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
    html = "<html><body>It is now %s.</body></html>" % now
    return HttpResponse(html)
```

让我们看看这个代码:

- 首先，我们从`django.http`模块导入了`HttpResponse`类，`datetime`库

- 接下来，我们定义了一个名为`current_datetime`的函数。这是视图函数，每个视图函数都接收一个HttpRequest对象作为第一个参数，通常称为请求`request`。
注意，视图函数的名称并不重要，它并没有以某种方式按顺序命名的Django来识别它。我们在这里调用`current_datetime`，因为这个名字清楚地表明它做什么。

- 此视图返回一个包含生成的响应的HttpResponse对象。每个视图函数负责返回HttpResponse对象。

> Django's Time Zone
> Django includes a TIME_ZONE setting that defaults to America/Chicago. This probably isn't where you live, so you might want to change it in your settings file.


<br/>
<br/>


### 映射URLs到视图

因此，要回顾一下，这个视图函数返回一个HTML页面，其中包括当前的日期和时间。要显示此视图的特定URL，你需要创建一个URLconf。


<br/>
<br/>


### 返回错误

在Django中返回HTTP error codes很容易。存在其它常见的HTTP状态码的HttpResponse子类(subclasses)。可在文档中找到所有可用的子类。

```py
from django.http import HttpResponse, HttpResponseNotFound

def my_view(request):
    # ...
    if foo:
        return HttpResponseNotFound('<h1>Page not found</h1>')
    else:
        return HttpResponse('<h1>Page was found</h1>')
```

没有对每个可能的HTTP响应码一个专门的子类，由于HttpResponse文档中，还可通过HTTP状态码到构造函数的HttpResponse创建你喜欢的任何状态码:

```py
from django.http import HttpResponse

def my_view(request):
    return HttpResponse(status=201)
```

由于404错误是目前最常见的HTTP错误，处理它有简单的方法。

<br>

**The Http404 exception**

class `django.http.Http404`

当你返回一个如HttpResponseNotFound的错误，你负责定义产生的错误页面的HTML:

```py
from django.http import Http404
from django.shortcuts import render
from polls.models import Poll

def detail(request, poll_id):
    try:
        p = Poll.objects.get(pk=poll_id)
    except Poll.DoesNotExist:
        raise Http404("Poll does not exist")
    return render(request, 'polls/detail.html', {'poll': p})
```

当Django返回一个404时，为了显示自定义的HTML，你可以创建一个名为 `404.html`的模板并放在模板树顶层。


<br/>
<br/>



### 自定义错误视图

Django的默认错误应该能满足大多数情况，但你也可以自定义行为。

```py
# page_not_found()视图会被 handler404覆盖
handler404 = 'mysite.views.my_custom_page_not_found_view'

# server_error()视图会被handler500覆盖
handler500 = 'mysite.views.my_custom_error_view'

# permission_denied()视图会被handler403覆盖
handler403 = 'mysite.views.my_custom_permission_denied_view'

# bad_request()会被handler400覆盖
handler400 = 'mysite.views.my_custom_bad_request_view'
```

<br>

> 参见
> Use the CSRF_FAILURE_VIEW setting to override the CSRF error view.

<br>


**测试自定义错误视图**

为了测试自定义错误处理程序的响应，提高在测试视图中的相应的异常:

```py
from django.core.exceptions import PermissionDenied
from django.http import HttpResponse
from django.test import SimpleTestCase, override_settings
from django.urls import path


def response_error_handler(request, exception=None):
    return HttpResponse('Error handler content', status=403)

def permission_denied_view(request):
    raise PermissionDenied


urlpatterns = [
    path('403/', permission_denied_view),
]

handler403 = response_error_handler


# ROOT_URLCONF must specify the module that contains handler403 = ...
@override_settins(ROOT_URLCONF=__name__)
class CustomErrorHandlerTests(SimpleTestCase):

    def test_handler_renders_template_response(self):
        response = self.client.get('/403/')
        # Make assertions on the response here. For example:
        self.assertContains(response, 'Error handler content', status_code=403)
```





<br/>
<br/>
<br/>





## 视图装饰器

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/http/decorators/>


Django提供了可应用于视图，以支持各种HTTP特征的几个装饰器(decorators)。


<br/>


### 允许HTTP方法

`django.views.decorators.http`中的装饰器可用于限制访问基于请求方法的视图。如果条件不具备装饰器会返回`django.http.HttpResponseNotAllowed`。

- `require_http_methods(request_method_list)`

```py
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET", "POST"])
def my_view(request):
    # I can assume now that only GET or POST requests make it this far
    pass
```

- `require_GET()`：装饰器要求视图只接受GET方法
- `require_POST()`：装饰器要求视图只接受POST方法
- `require_safe()`：装饰器要求视图只接受GET和HEAD方法。这些方法通常被认为是安全的的。


<br/>
<br/>


### 条件视图处理

装饰器`django.views.decorators.http`可用来控制特定视图的缓存行为。

- `condition(etag_func=None, last_modified_func=None)`
- `etag(etag_func)`
- `last_modified(last_modified_func)`

这些装饰器可用来生成ETag和Last-Modified headers。


<br/>
<br/>


### GZip压缩

装饰器`django.views.decorators.gzip`在每个视图上控制内容压缩。

- `gzip_page()`：如果浏览器允许使用gzip压缩，这个装饰器压缩内容。


<br/>
<br/>


### Vary Headers

装饰器`django.views.decorators.vary`可用于根据特定请求头来控制缓存。

- `vary_on_cookie(func)`
- vary_on_headers(`*headers`)


<br/>
<br/>


### 缓存

装饰器`django.views.decorators.chache`控制服务器和客户端的缓存。

- cache_control(`**kwargs`)：此装饰器加入所有的关键字参数给它修补响应的Cache-Control头
- `never_cache(view_func)`： 此装饰器添加`Cache-Control: max-age=0, no-cache, no-store, must-revalidate`头为响应指示页面不该被缓存。




<br/>
<br/>
<br/>




## 文件上传

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/http/file-uploads/>


Django处理文件上传时，文件最终会位于`attr:request.FILES<django.http.HttpRequest.FILES>`




<br/>
<br/>
<br/>




## 快捷函数

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/http/shortcuts/>


包`django.shortcuts`收集助手函数或跨堆积MVC的类。换句话说，为了方便起见，这些函数/类引入受控耦合。


<br/>


### render()

- `render(request, template_name, context=None, content_type=None, status=None, using=None)：将给定的模板与给定的上下文字典组合在一起，并以渲染的文本返回一个HttpResponse对象。

```
# 必选参数
request: 用于生成此响应的请求对象
template_name: 要使用的模板名称


# 可选参数
context: 要添加到模板上下文的值的字典
content_type 用于结果文档的MIME类型默认行为
status: 响应的状态码，默认200
using: 用于加载模板的模板引擎
```

栗子:

```py
from django.shortcuts import render

def my_view(request):
    # view code here
    return render(request, 'myapp/index.html', {'foo': 'bar'}, content_type='application/xhtml+xml')
```

此栗子相当于:

```py
from django.http import HttpResponse
from django.template import loader

def my_view(request):
    # view code here...
    t = loader.get_template('myapp/index.html')
    c = {'foo': 'bar'}
    return HttpResponse(t.render(c, request), content_type'application/xhtml+xml)
```


<br/>
<br/>


### redirect()

```
redirect(to, permanent=False, *args, **kwargs)
# 将一个HttpResponseRedirect返回传递到参数的适当URL

- A model: the model's get_absolute_url() function will be called.
- A view name, possibly with arguments: reverse() will be used to reverse-resolve the name.
- An absolute or relative URL, which will be used as-is for the redirect location.

By default issues a temporary redirect; pass permanent=True to issue a permanent redirect.
```

栗子:

你可以在多种方式中使用`redirect()`函数。

```py
# By passing some object; that object's get_absolute_url() method will be called to figure out the redirect URL
from django.shortcuts import redirect

def my_view(request):
    ...
    obj = MyModel.objects.get(...)
    return redirect(obj)


# By passing the name of a view and optionally some positional or keyword arguments; the URL will be reverse resolved using the reverse() method
def my_view(request):
    ...
    return ridirect('some-view-name', foo='bar')


# By passing a hardcoded URL to redirect to
def my_view(request):
    ...
    return redirect('/some/url/')
# This also works with full URLs
def my_view(request):
    ...
    return redirect('https://example.com/')


# 默认情况下，redirect()返回一个临时重定向。设置parmanent=True修改为永久
def my_view(request):
    ...
    obj = MyModel.objects.get(...)
    return redirect(obj, permanent=True)
```


<br/>
<br/>


### get_object_or_404()

```py
get_object_or_404(klass, *args, **kwargs)
# Calls get() on a given model manager, but it raises Http404 instead of the model's DoesNotExist exception.


# 必选参数
klass  # A Model class, a Manager, or a QuerySet instance from which to get the object.
**kwargs  # Lookup parameters, which should be in the format accepted by get() and filter().
```

栗子:

```py
from django.shortcuts import get_object_or_404

def my_view(reqeust):
    obj = get_object_or_404(MyModel, pk=1)


# 此栗相当于
from django.http import Http404

def my_view(request):
    try:
        obj = MyModel.objects.get(pk=1)
    execpt MyModel.DoesNotExist：
        raise Http404("No MyModel matches the given query.")
```


<br/>
<br/>


### get_list_or_404()

```
get_list_or_404(lkass, *args, **kwargs)
# Returns the result of filter() on a given model manager cast to a list, raising Http404 if the resulting list is empty.


# 必选参数
klass  # A Model, Manager or QuerySet instance from which to get the list.
**kwargs  # Lookup parameters, which should be in the format accepted by get() and filter().
```

栗子:

```py
# Get all published objects from MyModel
from django.shortcuts import get_list_or_404

def my_view(request):
    my_objects = get_list_or_404(MyModel, published=True)


# 此栗子相当于
from django.http import Http404

def my_view(reqeust):
    my_objects = list(MyModel.objects.filter(published=True))
    if not my_objects:
        raise Http404("No MyModel matches the given query.")
```




<br/>
<br/>
<br/>




## 通用视图

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/http/generic-views/>
docs: <https://docs.djangoproject.com/zh-hans/2.1/ref/class-based-views/>

<br/>


### 基本视图与通用视图

Base class-based views 可以被认为是父视图，其可以通过本身被使用，或者从继承。它们可能不提供所有项目所需要的功能，在这种情况下，有混入其中的扩至基视图可以做。

Django generic views are built off of those base views，被开发作为一个快捷功能，用于公共使用模式（如显示对象的详细信息）。They take certain common idioms and patterns found in view development and abstract them so that you can quickly write common views of data without having to repeat yourself.
大多数的通用视图需要`QuerySet`键——它是一个`QuerySet`实例。




<br/>
<br/>
<br/>




## 中间件

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/http/middleware/>
内检中间件: <https://docs.djangoproject.com/zh-hans/2.1/ref/middleware/>

中间件是Django 请求/响应 处理的钩子框架。它是一个轻量级的、低级的插件系统，用于全局改变Django的输入或输出。

每个中间件组件负责做一些特定的功能。如Django的一个中间件组件`AuthenticationMiddleware`，它使用会话将用于与请求关联起来。
文档中解释了中间件是如何工作的，如何激活中间件，如何编写自己的中间件。Django具有一些内置的中间件，你可以直接使用它们。




<br/>
<br/>
<br/>




## 如何使用会话

sessions: <https://docs.djangoproject.com/zh-hans/2.1/topics/http/sessions/>


Django是支持匿名会话的。会话框架允许你基于每个站点访问者存储和检索任意数据。它在服务端存储数据并提供cookie的发送和接收。
Cookie包含会话ID，而不是数据本身。


<br/>


### 打开会话

会话通过配置一个中间件(`django.contrib.sessions.middleware.SessionMiddleware`)实现。




<br/>
<br/>

---

<br/>
<br/>




# 使用表单

docs: <https://docs.djangoproject.com/zh-hans/2.1/topics/forms/>


介绍Web表单的基本内容以及它们在Django中是如何处理的。

除非搭建的网站和应用只发布内容而不接收访问者的输入，否则你就需要理解和使用表单。
Django提供了一系列的工具和库来帮助构建表单来接收网站访问者的输入，然后处理以及响应这些输入。


<br/>


## HTML表单

在HTML中，表单是在`<form>...</form>`中的一些元素，它允许访客做一些类似输入文档、选择选项、操作对象或空间等动作，然后发送这些信息到服务端。

一些表单界面元素(文本框或复选框)非常简单并内置在HTML中。其它会复杂些，如弹出日期选择、允许你移动滑块或操作控件，一般通过使用JavaScript，CSS以及HTML表单中的`<input>`元素来实现这些效果。

表单必须指定两样东西:

- 何地: 负责响应用户输入数据的URL地址
- 如何: 数据如何请求使用的HTTP方法

```
例如，Django admin登录表单包含了一些<input>元素：
用户名用type="text"，密码用type="password"，登录按钮用type="submit"。
它还包含一些用户看不到的隐藏文本字段，Django用它们来决定下一步行为。

它还告诉浏览器表单数据应该发往<form>的action属性指定的URL——`/admin/`，并且应该使用method属性指定的HTTP方法——post。

当<input type="submit" value="Log in">元素被触发的时候，数据会发送到`/admin/`。
```

<br/>

**GET和POST**

处理表单时只会用到GET和POST两种方法。
Django的登录表单使用POST方法传输数据，在这个方法中浏览器会封装表单数据，为了传输会进行编码，然后发送到服务端并接收它的响应。

相比之下，GET方法将提交的数据捆绑到一个字符串中，并用它来组成一个URL。该URL包含了数据要发送的地址以及一些键值对应的数据。如`https://docs.djangoproject.com/search/?q=forms&release=1`

任何可用于更改系统状态的请求都应该使用POST。
GET方法也不适合密码表单，因为密码会出现在URL中，也是也会出现在浏览器的历史记录以及服务器的日志中，而且都是以纯文本都形式。它也不适合处理大量的数据或者二进制数据。在Web应用的管理表单中使用GET请求具有安全隐患：攻击者很容易通过模拟请求来访问系统的敏感数据。POST方法通过与其它像CSRF这样的保护措施配合使用，能对访问提供更多控制。


<br/>
<br/>


## Django在表单中的角色

处理表单是一件挺复杂的事情。许多不同的数据可能在一张表单中准备显示，渲染成HTML，使用方便的界面进行编辑，传到服务器，验证和清理数据，然后保存或跳过进行下一步处理。

Django会处理设计表单的三个不同部分:

- 准备并重组数据，以便下一步的渲染
- 为数据创建HTML表单
- 接收并处理客户端提交的表单和数据

你可以手动编写代码来实现，但Django可以帮你完成所有这些工作。


<br/>
<br/>


## Django中的表单

Web应用中所说的表单，可能指的是HTML `<form>`，或者是生成了它的Django Form，再或者是提交时返回的结构化数据，亦或是这些端到端作业的合集。

<br>

**Django的Form类**

Django表单系统的核心组件是Form类。它与Django模型描述对象的逻辑结构、行为以及它呈现给我们内容的形式的方式大致相同，Form类描述一张表单并决定它如何工作及呈现。
类似于模型类的字段映射到数据库字段的方式，表单类的字段会映射到HTML表单的`<input>`元素。ModelForm通过Form映射模型类的字段到HTML表单的`<input>`元素。
表单字段本身也是类，他们管理表单数据并在提交表单时执行验证。DateField和FileField处理的数据类型差别很大，所以必须用来处理不同的字段。
在浏览器中，表单字段以HTML控件的形式展示给我们。每个字段类型都有与之相匹配的控件类，但必要时可以覆盖。

<br>

**实例化、处理和渲染表单**

在Django中渲染一个对象的时候，我们通常：

- 在视图中获取它
- 将它传递给模板上下文
- 使用模板变量将它扩展为HTML标记

在模板中渲染表单几乎与渲染任何其他类型的对象的一样，但是存在一些关键性的差异。
如果模型实例不包含数据，在模板中对它做任何处理几乎没什么用。但完全有理由用来渲染一张空表单——当我们希望用户来填充的时候就会这么做。
所以当我们在视图中处理模型实例时，我们一般从数据库中获取它。当我们处理表单时，我们一般在视图中实例化它。

当我们实例化表单时，我们可以选择让它为空或者对它预先填充：

- 来自已经保存的模型实例的数据
- 从其它来源获取的数据
- 从前面一个HTML表单提交过来的数据


<br/>
<br/>


## 构建一张表单

**需要完成的工作**

假设你希望在你的网站上创建一张简易的表单，来获取用户的名字：

```jinja2
<form action="/your-name/" method="post">
    <label for="your_name">Your name: </label>
    <input id="your_name" type="text" name="your_name" value="{{ current_name }}">
    <input type="submit" value="OK">
</form>
```

这告诉浏览器将表单数据返回给URL `/your-name/`，并使用POST方法。它将显示一个标签为"Your name:"的文本字段，以及一个"OK"按钮。如果模板上下文包含一个`current_name`变量，它会被预填充到`your_name`字段。
你需要一个视图来渲染这个包含HTML表单的模板，并能适当提供`current_name`字段。
提交表单时，发送给服务器的POST请求将包含表单数据。

现在，你还需要一个与该`/your-name/` URL相对应的视图，该视图将在请求中找到相应的键值对，然后对其进行处理。

<br>

**在Django中构建一张表单**

- Form类

```py
# forms.py
from django import forms

class NameForm(forms.Form):
    your_name = forms.CharField(label='Your name', max_length=100)


# Form表单实例有一个`is_valid()`方法，它运行所有的字段验证。当此方法被调用时，如果所有字段包含有效数据，它将会：
# - 返回True
# - 将表单数据放到它的cleaned_data属性中
```

这样整个表单在第一次渲染时，会显示如下:

```
<lable for="your_name">Your name: </label>
<input id="your_name" type="text" name="your_name" maxlength="100" required>
```

注意它没有包含`<form>`标签和提交按钮。我们必须在模板中提供。

<br>

- 视图

发回Django网站的表单数据由视图来处理，一般和发布这个表单用的是同一个视图。这允许我们重用一些相同的逻辑。

为了处理表单，我们需要将它实例化到我们希望发布的URL的对应的视图中：

```py
# views.py

from django.http import HttpResponseRedirect
from django.shortcuts import render

from .forms import NameForm

def get_name(request):
    # if this is a POST request we need to process the form data
    if request.method == 'POST':
        # create a form instance and populate it with data from the request
        form = NameForm(request.POST)
        # check whether it's valid
        if form.is_valid():
            # process the data in form.cleaned_data as required
            # ...
            # redirect to a new URL
            return HttpResponseRedirect('/thanks/')

    else:
        form = NameForm()

    return render(request, 'name.html', {'form': form})
```

如果我们访问这个视图用的是GET请求，它会创建一个空的表单实例并将其放置在模板上下文中进行渲染。
如果表单提交用的是POST请求，那么该视图再次创建一个表单实例并使用请求中的数据填充它。`form=NameForm(request.POST)`，这叫绑定数据到表单。
调用表单的`is_valid()`方法，如果不为True，就带着表单返回到模板。这次表单不在为空，所以HTML表单将用之前提交的数据进行填充，放到可以根据需要进行编辑和修正的位置。
如果`is_valid()`为True，我们就能在cleaned_data属性中找到所有通过验证的表单数据。我们可以发送一个HTTP重定向告诉浏览器下一步去向之前用这些数据更新数据库或做其它处理。

<br>

- 模板

没有必要在模板`name.html`中做过多的操作。举个栗子:

```jinja2
<form action="/your-name/" method="post">
    {% csrf_token %}
    {{ form }}
    <input type="submit" value="Submit">
</form>
```

所有的表单字段机器属性都将通过Django模板语言从 `{{ form }}` 中被解包成HTML标记。

> 表格和跨站请求伪造保护
> Django自带一个简单易用的跨站请求伪造保护。当通过POST方法提交一张启用了CSRF防护的表单时，你必须使用上例中这样的模板标签 csrf_token。但是，由于CSRF防护在模板中没有与表单直接绑定，因此这个标签在本页文档之后的示例中都将被忽略。

现在我们有了一个可以工作的Web表单，它通过一张Django Form描述，由一个视图来处理并渲染成一个HTML `<form>`。


<br/>
<br/>


## 详解Django的Form类

所有表单都作为`django.forms.Form`或者`django.forms.ModelForm`的子类来创建。

如果你的表单是要直接用来添加或编辑Django模型，用ModelForm，可以省时省力省代码，因为它会根据Model类构建一张对应字段及其属性的表单。

<br>

**绑定和未绑定的表单实例**

bound和unbound forms之间的区别非常重要：

- 未绑定的表单没有与其关联的数据。当渲染给用户时，它会是空的或者包含默认值。
- 绑定的表单拥有已提交的数据，因此可用来判断数据是否合法。

表单的`is_bound`属性将告诉你一张表单是否具有绑定的数据。

<br>


**字段详解**

栗子:

```py
# forms.py
from django import forms

class ContactForm(forms.Form):
    subject = forms.CharField(max_length=100)
    message = forms.CharField(widget=forms.Textarea)
    sender = forms.EmailField()
    cc_myself = forms.BooleanField(required=False)
```

- 控件

每个表单字段都有一个相对应的控件类，这个控件类又有对应的HTML表单控件，比如`<input type="text">`。

- 字段数据

无论用表单提交了什么数据，一旦通过调用`is_valid()`验证成功，已验证的表单数据将被放到`form.cleaned_data`字典中。这里的数据已经很好的为你转化为Python类型。如cc_myself会被转化成一个布尔值。同样的，字段 IntegerField 和 FloatField 的值分别会被转化为Python的 int 和 float 类型。

栗子:

```py
# views.py
from django.core.mail import send_mail

if form.is_valid():
    subjetct = form.cleaned_data['subject']
    message = form.cleaned_data['message']
    sender = form.cleaned_data['sender']
    cc_myself = form.cleaned_data['cc_myself']

    recipients = ['info@example.com']
    if cc_myself:
        recipients.append(sender)

    send_mail(subject, message, sender, recipients)
    return HttpResponseRedirect('/thanks/')
```


<br/>
<br/>


## 使用表达模板

你只需要将表单实例放到模板的上下文中即可。

<br>

**表单渲染选项**

> 额外表单模板标签
> 不要忘记，一张表单的输出不包含外层`<form>`标签以及submit控件。这些必须由你自己提供。

对于`<label>`, `<input>`对，还有其它输出选项：

- ```{{ form.as_table }}```将渲染它们作为表格包裹在`<tr>`标记中
- ```{{ form.as_p }}```将渲染它们包裹在`<p>`标记中
- ```{{ form.as_ul }}```将渲染它们包裹在`<li>`标记中

注意，你必须自己提供外层的`<table>`或`<ul>`元素。




<br/>
<br/>

---

<br/>
<br/>




# 模板

templates: <https://docs.djangoproject.com/zh-hans/2.1/topics/templates/>


作为一个Web框架，Django需要一种动态生成HTML的便捷方法。最常用的方法依赖于模板。模板包含所需HTML输出的静态部分以及描述动态内容将被插入的一些特殊语法。

Django项目可以配置一个或多个模板引擎（或者不使用模板引擎）。Django后端内置一个自己的模板系统，创造性地称为Django Template Language(DTL)。后端也可以使用第三方提供的其它可用的模板语言。
Django template language是Django自己的模板系统。这是一个很好的模板库，即使它是相当僵硬和使用时带有它自己特质。如果你没有紧迫的理由需要去选择另一个后端，则应该使用DTL。

Django定义了一个标准的API，用于加载和渲染模板，而不用考虑后端的模板系统。加载包括查找给定标识符的模板并对其进行预处理，通常将其编译的结果保存在内存中。渲染工具将上下文数据插入模板并返回结果字符串。


<br/>


## 模板引擎的支持

```py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
        # ... some options here ...
        },
    },
]


# BACKEND: 实现Django模板后台API，内建后台有:
# - django.template.backends.django.DjangoTemplates
# - django.template.backends.jinja2.Jinja2
# DIRS: 定义一个目录列表，其中模板引擎应该寻找的源文件
# APP_DIRS: 告诉引擎是否应该在安装的应用内寻找模板
```




<br/>
<br/>

---

<br/>
<br/>




# Django Template Language

参考:

- <https://docs.djangoproject.com/zh-hans/2.1/topics/templates/>
- <https://docs.djangoproject.com/zh-hans/2.1/ref/templates/language/>


Django模板引擎提供了一种强大的mini-language，用于定义应用程序的面向用户层，鼓励应用程序和表示逻辑的清晰分离。任何了解HTML的人都可以维护模板，不需要Python的知识。

Django模板语言其实和Jinja2类似，只不过Jinja2更自由些。


<br/>


## Django模板语言

本文档解释了Django模板系统的语言语法。
Django的模板语言只在功能和易用性之间取得平衡。它旨在让那些习惯使用HTML的人感到舒服。如果你对其它模板语言(Smart, Jinja2)有任何接触，那么您应该对Django的模板感到宾至如归。

> 哲学
> Django模板系统不仅仅是嵌入到HTML中的Python。模板系统用于表示，而不是程序逻辑。
> Django模板系统提供的tags功能与某些编程结构类似——`if`标签用于布尔测试，`for`标签用于循环...但这些并不是简单地作为相应的Python代码执行，并且模板系统不会执行任意Python表达式。


<br/>
<br/>


## 模板

Templates


一个模板是一个简单的文本文件。它可以生成任何基于文本的格式(HTML, XML, CSV...)。

模板包含变量(variables)，这些变量在评估模板时将替换为值，而变量则包含控制模板逻辑的标签(tags)。

下面是一个最小的模板示例，每个元素将在后面解释。

```jinja2
{% extends "base_generic.html" %}

{% block title %}{{ section.title }}{% endblock %}

{% block content %}
<h1>{{ section.title }}</h1>

{% for story in story_list %}
<h2>
  <a href="{{ story.get_absolute_url }}">
      {{ story.headline|upper }}
  </a>
</h2>
<p>{{ story.tease|truncatewords:"100" }}</p>
{% endfor %}
{% endblock %}
```

> 为什么使用基于文本而不是基于XML的模板？我们希望Django的模板语言不仅可用于XML/HTML模板。在互联网上，我们将其用于电子邮件、JS和CSV。你可将模板语言用于任何基于文本的格式。
> 让人类编辑XML是虐待狂!


<br/>
<br/>


## 变量

Variables


变量像这样: ```{{ variable }}```。当模板引擎遇到变量时，它会计算该变量并将其替换为结果。变量名由字母、数字和下划线组成，但不能以下划线开头。点(`.`)也出现在变量部分，尽管它具有特殊含义。重要的是，变量名称中不能包含空格或标点符号。

从技术上来说，当模板系统遇到一个点时，它会按照一下顺序尝试查找:

- 字典
- 属性或方法
- 数字索引

在上面的例子中，```{{ section.title }}```将替换为section对象的title属性。
如果使用不存在的 变量，模板系统将插入`srting_if_invalid`选项的值，默认情况下设置为空`''`。

请注意，模板表达式```{{ foo.bar }}```中的bar将被解释为文字字符串，而不使用变量bar的值(如果上下文中存在)。

可能无法访问以下划线开头的变量属性，因为它们通常被视为私有。


<br/>
<br/>


## 过滤器

Filters


你可使用过滤器(filters)修改要显示的变量。
过滤器像这样: ```{{ name|lower }}```。这会在通过小写过滤器后显示变量的值，后者将文本转换为小写。使用管道(`|`)应用过滤器。

过滤器可以链接，一个过滤器的输出应用于下一个过滤器。```{{ text|escape|linebreaks }}```。```{{ text|escape|linebreaks }}```是转义文本内容，然后将换行符转换为`<p>`标签的常用习惯写法。

一些过滤会使用参数，过滤器参数如下所示: ```{{ bio|truncatewords:30 }}```，这将显示变量bio的前30个单词。
过滤参数包含的空格必须使用引号引用，如: ```{{ list|join:", " }}```。

Django提供了大约60个内置模板过滤器。可在[built-in filter reference](https://docs.djangoproject.com/zh-hans/2.1/ref/templates/builtins/#ref-templates-builtins-filters)查看全部。

以下是一些常用的模板过滤器:

- default
如果变量为false或者为空，使用给定的默认值。

```jinja2
{{ value|default:"nothing" }}
```

- length
返回值得长度，使用与strings和lists。

```jinja2
{{ value|length }}
```

- filesizeformat
将值格式化为人类可读的文件大小(`12KB`, `2MB`...)

```jinja2
{{ value|filesizeformat }}
```

当然，你可以创建自己的自定义模板过滤器。


<br/>
<br/>



## 标记

Tags


标记像这样: ```{% tag %}```。标签比变量更复杂：有些在输出中创建文本，有些通过执行循环或逻辑来控制流，有些则将外部信息加载到模板中以供以后的变量使用。

有些标签需要开始和结束标记: ```{% tag %}```...```{% endtag %}```。

Django附带了大约24个内置模板标签。可查看[build-in tag reference](https://docs.djangoproject.com/zh-hans/2.1/ref/templates/builtins/#ref-templates-builtins-tags)。

下面是一些常用的标记:

- for
循环遍历数组中的每个项。

```jinja2
<ul>
{% for athlete in athlete_list %}
    <li>{{ athlete.name }}</li>
{% end for %}
</ul>
```

- if, elif, else

```jinja2
{% if athlete_list %}
    Number of athletes: {{ athlete_list|length }}
{% elif athlete_in_locker_room_list %}
    Athletes should be out of the locker room soon!
{% else %}
    No athletes.
{% enfif %}
```

可在if标记中使用过滤器和操作符:

```jinja2
{% if athlete_list|length > 1 %}
    Team: {% for athlete in athlete_list %} ... {% endfor %}
{% else %}
    Athlete: {{ athlete_list.0.name }}
{% endif %}
```

- block, extends
设置模板继承[template inheritance](https://docs.djangoproject.com/zh-hans/2.1/ref/templates/language/#id1)，这是一种在模板中减少样板(boilerplate)的强大方法。

你可以创建自己的自定义模板标记。


<br/>
<br/>


## 注释

Comments


注释的语法为: ```{# #}```。

例如，此模板将呈现为hello: ```{# greeting #}hello```。


<br/>
<br/>


## 模板继承

Templates inheritance


Django模板引擎最强大，也是最复杂的就是模板继承。模板继承允许你构建一个基础骨架模型，其中包含站点的所有常用元素(elements)，并定义子模块可以覆盖的块(block)。

理解模板继承的栗子(`base.html`):

```jinja2
<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="stylesheet" href="style.css">
    <title>{% block title %}My amazing site{% endblock %}</title>
</head>

<body>
    <div id="sidebar">
        {% block sidebar %}
        <ul>
            <li><a href="/">Home</a></li>
            <li><a href="/blog/">Blog</a></li>
        </ul>
        {% endblock %}
    </div>

    <div id="content">
        {% block content %}{% endblock %}
    </div>
</body>
</html>
```

定义了一个简单的HTML框架文档，你可将其用于简单的双列页面。子模板的工作是用内容填充空块(empty block)。
在此示例中，块标记(block)定义了子模块可以填充的三个块。所有块标记的作用是告诉模板引擎子模块可以覆盖模板的这些部分。

子模板栗子:

```jinja2
{% extends "base.html" %}

{% block title %}My amazing blog{% endblock %}

{% block content %}
{% for entry in blog_entries %}
    <h2>{{ entry.title }}</h2>
    <p>{{ entry.body }}</p>
{% endfor %}
{% endblock %}
```

扩展标记(extends)是这里的关键。它告诉模板引擎改模板扩展另一个模板。当模板系统评估此模板时，它首先找到父模板。

此时，模板引擎会注意到`base.html`中的三个块标记，并将这些块替换为子模板的内容。输出可能如下:

```jinja2
<!DOCTYPE html>
<html lange="en">
<head>
    <link rel="stylesheet" href="style.css">
    <title>My amazing blog</title>
</head>

<body>
    <div id="sidebar">
        <ul>
            <li><a href="/">Home</a></li>
            <li><a href="/blog/">Blog</a></li>
        </ul>
    </div>

    <div id="content">
        <h2>Entry one</h2>
        <p>This is my first entry.</p>

        <h2>Entry two</h2>
        <p>This is my second entry.</p>
    </div>
</body>
</html>
```

请注意，由于子模块未定义sidebar块，因此将使用父模板中的值。

<br>

你可以根据需要使用尽可能多的继承级别。使用继承的一种常见方法是以下三级(three-level)方法:

- 创建`base.html`基础模板，其中包含网站的主要外观
- 为网站的每个部分(section)创建一个`base_SECTIONNAME.html`模板，这些模板都扩展基础模板，并包含特定于部分的样式设计
- 为每种类型的页面创建单独的模板，这些模板扩展了相应部分的模板

这种方法可以最大化代码重用，并且可以轻松地将项目添加到共享内容区域。

<br>

以下是使用继承的一些技巧与提示:

- 如果在模板中使用```{% extends %}```，则它必须是该模板中的第一个模板标记。否则，模板继承不起作用。
- 基础模板中的```{% block %}```标记越多越好。请记住，子模块不必定义所有父块，因此你可在多个块中填写合理的默认值，然后仅定义需要的块。
- 如果在许多模板中复制了内容，则可能意味着你应该将内容移动到父模板中的```{% block %}```。
- 如果需要从父模板获取块的内容，```{{ block.super }}```变量将起作用。如果要添加到父模块的内容而不是完全覆盖它，这将非常有用。
- 使用模板标记`as`语法在```{% block %}```块之外创建的变量不能在块内使用。

```jinja2
{% trans "Title" as title %}
{% block conten %}{{ title }}{% endblock %}
```

- 为了提高可读性，可选择为```{% endblock %}```标记指定名称。在较大的模板中，此技术可帮助查看正在关闭的块标记。

```jinja2
{% block content %}
...
{% endblock conten %}
```

最后，请注意，你无法在同一模板中定义多个名称相同的块标记。


<br/>
<br/>


## 自动HTML转义

Automatic HTML escaping


当从模板生成HTML时，变量将始终存在影响生成的HTML的字符的风险。

考虑这个模板片段:
```
Hello {{ name }}
```

首先，这似乎是一种显示用户名的无害方式。但考虑如果用户输入其名称会发生什么: `<script>alert('hello')</script>`
使用此名称值，模板将呈现为: `Hello, <script>alert('hello')</script>`
这意味着浏览器会弹出一个JS警告框！

类似地，如果名称包含`<`符号: `<b>username`
这将导致像这样的渲染模板: `Hello, <b>username`
反过来，这将导致网页的其余部分被加粗。

显然，用户提交的数据不应盲目信任并直接插入到你的网页中，因为恶意用户可能会利用这种漏洞来做坏事。此类安全漏洞称为跨站点脚本(XSS, cross site scripting)攻击。

要避免此问题，有两种选择:

- 一，可以确保通过转义过滤器运行每个不受信任的变量，该过滤器可将可能有害的HTML字符转换为无害的HTML字符。这是Django最初几年的默认解决方案，但问题在于它让你有责任确保你逃避一切。
- 二，可利用Django的自动HTML转义功能。

默认情况下，Django中的每个模板都会自动转义每个变量标记的输出。具体来说，这五个字符被转义:


- `<`被转换为`&lt`;
- `>`被转换为`&gt`;
- `'`被转换为`&#39`;
- `"`被转换为`&quot`;
- `&`被转换为`&amp`;


同样，我们强调默认情况下已启用此行为。如果你正在使用Django的模板系统，那么你将受到保护。


<br/>
<br/>


### 如何关闭

如果不想自动转义数据，则可通过多种方式将其关闭。

- 单独的变量(individual bariables)
使用`safe`过滤器。

```jinja2
This will be escaped: {{ data }}
This will not be escaped: {{ data|safe }}
```

- 模板块(template block)
将模板包装在`autoescape`标记中。它将on或off作为其参数。

```jinja2
{% autoescape off %}
    Hello {{ name }}
{% endautoescape %}
```


<br/>
<br/>


### 字符串文字和自动转义

String literals and automatic escaping

如前面所说，过滤参数可以是字符串:
```
{{ data|default:"This is a string literal." }}
```



<br/>
<br/>



## 访问方法调用

Accessing method calls


Most method calls attached to objects are also available from within templates. 这意味着，模板必须比类属性和从视图中传递的变量获得更多的访问。

```jinja2
{% for comment in task.comment_set.all %}
    {{ comment }}
{% endfor %}
```

你可以轻松访问你明确对自己的模型定义的方法:

```py
# models.py
class Task(models.Model):
    def foo(self):
        return "bar"
```

```jinja2
{{ task.foo }}
```


<br/>
<br/>


## 自定义tag和filter库

某些应用程序提供了自定义的标签和过滤器库。要访问这些模板，确保应用在`INSTALLED_APPS`中，然后在模板中使用`load`标记.

```jinja2
{% load humanize %}
{{ 45000|intcomma }}
```


load标记载入humanize标记库，然后就可以使用intcomma过滤器。

load标记可载入多个:
```
{% load humanize i18n %}
```
























<br/>
<br/>

---

<br/>
<br/>



















# 基于类的视图

class-based-views: <https://docs.djangoproject.com/zh-hans/2.1/topics/class-based-views/>


视图是可调用的，能接受用户的请求并返回响应。视图远不止是个函数，Django提供了一些可用作视图的类的示例，允许你通过继承和复用构建自己的视图并且复用这些代码。


<br/>


## 基于类的视图

intro: <https://docs.djangoproject.com/zh-hans/2.1/topics/class-based-views/intro/>


基于类的视图提供了实现视图作为Python对象来替代函数。相比基于函数的视图，它们不取代基于函数的视图，但有一定的差异和优势：

- Organization of code related to specific HTTP methods (GET, POST, etc.) can be addressed by separate methods instead of conditional branching.
- Object oriented techniques such as mixins (multiple inheritance) can be used to factor code into reusable components.









<br/>
<br/>
<br/>






## 内置的基于类的通用视图

Built-in class-based generic views: <https://docs.djangoproject.com/zh-hans/2.1/topics/class-based-views/generic-display/>


编写Web应用程序可以是单调的，因为我们一次又一次地重复某些模式。Django视图带走一些在模型和模板层千篇一律的单调，但Web开发者也在视图层级遇到了这些无聊的单调。

Django的通用视图被开发来缓解此问题。他们采取的在视图中开发和抽象的发现，让你能快速地编写普通视图，而无需编写大量代码。
我们我们可以发现一些常见的任务，比如显示对象的列表，并编写任何对象列表的代码。然后有问题的模型可以作为一个额外的参数传递到URLconf。

Django自带的通用视图能做到以下几点：

- 为单个对象显示列表和详细页面。如果我们创建一个应用程序来管理会议，那么TalkListView和RegisteredUserListView就应是列表视图的栗子。一个单一的talk page是我们成detail view的例子。
- 在基于日期(`year/month/day`)归档页的对象，associated detail, and "latest" pages
- 允许用户创建、更新和删除对象——是否授权

总之，这些视图提供了方便的接口来执行最常见的任务，以帮助开发者解决遇到的问题。














<br/>
<br/>
<br/>




## 基类视图和表单处理

Form handling with class-based views: <https://docs.djangoproject.com/zh-hans/2.1/topics/class-based-views/generic-editing/>


表单处理通常有3条路径：

- Initial GET (blank or prepopulated form)
- POST with invalid data (typically redisplay form with errors)
- POST with valid data (process the data and typically redirect)

实现此常常导致自己有大量重复的样板代码。为了帮助避免此情况，Django提供的通用基于类视图的集合来处理表单处理。











<br/>
<br/>
<br/>




## 基类视图混入

Using mixins with class-based views: <https://docs.djangoproject.com/zh-hans/2.1/topics/class-based-views/mixins/>


> 警告
> 这是一个高级的话题。

Django内置的基类视图提供了许多功能，但其中某些你可能要分开使用。例如，你可能想编写一个视图，来渲染模板生成HTTP response，但是你不能使用TemplateView。或许你需要渲染仅POST模板，GET是另外一回事。虽然你可直接使用TemplateResponse，这将有可能导致重复的代码。

出于这个原因，Django还提供了许多离散功能的混入。模板渲染，例如，被封装在TemplateResponseMixin。













<br/>
<br/>
<br/>




## 例子

Django提供了适合广泛应用的基视图类。所有视图从View class继承，它处理链接到URLs的视图，HTTP方法调度等简单功能的视图继承。RedirectView是一个简单的HTTP重定向，并且TemplateView扩展基类使其也呈现渲染的模板。




<br/>
<br/>
<br/>




## 简单使用

使用通用视图组件但的方法是直接在URLconf中创建它们。如果你只改变一个基类视图的几个简单属性，你可以简单地将它们传递到`as_view()`方法来调用自身：

```py
from django.urls import path
from django.views.generic import TemplateView

urlpatterns = [
    path('about/', TemplateView.as_view(template_name="about.html")),
]
```

传递给`as_view()`的任何参数将覆盖在类中设置的属性。在此例中，在TemplateView设置`template_name`。类似的覆盖模式可用于RedirectView的url属性。




<br/>
<br/>
<br/>




## 子类通用视图

第二种，更强大的方式去使用通用视图是从现有视图继承和在子类中提供新值或方法覆盖属性或方法。

```py
# some_app/views.py
from django.views.generic import TemplateView

class AboutView(TemplateView):
    template_name = "about.html"

```

```py
# urls.py
from django.urls import path
from some_app.views import AboutView

urlpatterns = [
    path('aboug/', AboutView.as_view()),
]
```


<br/>


### 支持其它HTTP方法

```py
from django.urls import path
from books.views import BookListView

urlpatterns = [
    path('books/', BookListView.as_view()),
]
```

```py
from django.http import HttpResponse
from django.views.generic import ListView
from books.models import Book


class BookListView(ListView):
    model = Book

    def head(self, *args, **kwargs):
        last_book = self.get_queryset().latest('publication_date')
        response = HttpResponse('')
        # RFC 1123 date format
        response['Last-Modified'] = last_book.publication_date.strftime('%a, %d %b %Y %H:%M:%S GMT')
        return response
```






















<br/>
<br/>

---

<br/>
<br/>






























# 迁移

Migrations: <https://docs.djangoproject.com/zh-hans/2.1/topics/migrations/>


迁移是将你对模型的改变传播到你的数据库架构的Django方法。它被设计的目的主要是自动，但你需要知道什么时候才能迁移。当运行它们，你可能会遇到常见的问题。


<br/>


## 命令

与迁移和Django处理数据库交互的几个命令：

- `migrate`：负责applying和unapplying migrations
- `makemigrations`: 负责创建一个基于你对你的模型所做的更改的新的迁移
- `sqlmigrate`: 显示一个迁移的SQL语句
- `showmigrations`: 列出项目迁移及其状态

你应该考虑迁移为你的数据库模式的版本控制系统。`makemigrations`是负责包装你的模型改变为个体迁移文件——类似于提交(commit)。`migrate`是负责应用(applying)你的数据库。
每个应用程序的迁移文件位于该应用程序内部的`migrations`目录，设计于commit和sidtributed，他是代码库的一部分。你应该在你的开发机上运行它们一次，然后在你同事的机器上运行相同的迁移，并最终在生产机上运行。




<br/>
<br/>
<br/>




## 后端支持

迁移支持Django的所有后端。

- PostgreSQL
- MySQL
- SQLite




<br/>
<br/>
<br/>




## 工作流

迁移的工作很简单。修改模型，然后运行`makemigrations`:

```
python manage.py makemigrations
```

你的模型将被扫描，并且与当前包含在迁移文件的版本进行比较，然后一组新的迁移会被写出来。请务必阅读`makemigrations`的输出，它并不完美，对于复杂的变化可能无法检测到你所期望的那样。

一旦有了新的迁移文件，你应该把它们应用到你的数据库，以确保它们达到预期效果:

```
python manage.py migrate
Operations to perform:
  Apply all migrations: books
Running migrations:
  Rendering model states... DONE
  Applying books.0003_auto... OK
```

一旦迁移已应用，提交迁移和模型改变到你的版本控制系统作为一个单个提交(singel commit)。这样，当其他开发者check out代码时，他们将同时获得模型的改变并在同一时间执行迁移。
你可以为迁移取一个有意义的名称:

```
python manage.py makemigrations --name changed_my_model your_app_label
```



<br/>


### 版本控制

由于迁移是存储在版本控制中，你会偶尔遇到你和另一个开发人员在同一个应用上都有提交一个迁移，导致两个开发人员的迁移有相同迁移编号。
别担心，这些数字编号只是在开发者那里参考，Django只关注每个迁移有一个不同的名称。迁移指定它们依赖于哪些迁移——包括同一应用前面的迁移。

发生这些情况时，Django会提示你，给你一些选项。



<br/>
<br/>
<br/>




## 依赖

虽然迁移的每个程序，通过你的模型隐含的表和关系太复杂，无法在同一时间仅一个程序创建。
当你依赖别的东西来运行迁移，所产生的迁移将包含在迁移的依赖上。

在限制单一应用的依赖行为为影响大部分迁移操作。限制到一个单一应用(`makemigrations` or `migrate`)是尽力的承诺，而不是保证。需要任何其它程序使用，以获得正确的依赖。




<br/>
<br/>
<br/>




## 迁移文件

迁移存储为磁盘上的格式，这里成为迁移文件(migration file)。这些文件实际上是商定布局和声明样式的普通的Python文件。

一个基本的迁移文件:

```py
from django.db import migrations, models

class Migration(migrations.Migration):

    dependencies = [('migrations', '0001_initial')]

    operations = [
        migrations.DeleteModel('Tribble'),
        migrations.AddField('Author', 'rating', models.IntegerField(default=0)),
    ]

```




<br/>
<br/>
<br/>




## 向应用添加迁移

向新应用添加迁移很简单，一旦你做了一些改动，只需运行`makemigrations`。

如果你的应用已经有模型和数据库表，并且没有迁移。你需要转化它使用迁移，一个例子: `python manage.py makemigrations your_app_label`
这将为你的应用执行一个新的初始迁移。现在，执行`python manage.py migrate --fake-initial`，Django会检测你有一个初始迁移，它想创建的表已经存在，并将标识这些迁移为已经应用。

注意，这仅适用于给定的两件事：

- 你创建了表但没有改变你的模型。
- 你没有手动编辑你的数据库——Django将无法检测到与模型不匹配的数据库，你只得到迁移错误时尝试修改这些表。




<br/>
<br/>
<br/>




## 历史模型

当运行迁移时，Django是从存储在迁移文件中的模型的历史版本进行工作。




<br/>
<br/>
<br/>




## 数据迁移

与改变数据库模式一样，你也可以使用迁移数据库本身的数据。这通常称为数据迁移，最好把它写成单独地迁移，刚在模型架构迁移的旁边。
Djaong不能为你自动生成数据迁移。

首先，生成一个空的迁移文件:

```py
python manage.py makemigrations --empty yourappname
```

接着，打开此文件:

```
# Generated by Django A.B on YYYY-MM-DD HH:MM
from django.db import migrations

class Migration(migrations.Migration):

    dependencies = [
            ('yourappname', '0001_initial'),
    ]

    operations = [
    ]
```




<br/>
<br/>

---

<br/>
<br/>





























# 管理文件

Managing files: <https://docs.djangoproject.com/zh-hans/2.1/topics/files/>


此文档描述那些由用户上传的Django的文件的访问API。

























<br/>
<br/>

---

<br/>
<br/>



















































# 测试

Testing in Django: <https://docs.djangoproject.com/zh-hans/2.1/topics/testing/>























<br/>
<br/>

---

<br/>
<br/>





































# 用户认证

Django Auth: <https://docs.djangoproject.com/zh-hans/2.1/topics/auth/>


Django自带了一个用户认证系统。它处理用户账户、组、权限和基于cookie的用户会话。这一部分文档介绍了如何实现开箱即用。

















<br/>
<br/>

---

<br/>
<br/>


















# 缓存框架

Cache framework: <https://docs.djangoproject.com/zh-hans/2.1/topics/cache/>

























<br/>
<br/>

---

<br/>
<br/>


























# 条件视图处理

Conditional view processing: <https://docs.djangoproject.com/zh-hans/2.1/topics/conditional-view-processing/>


















<br/>
<br/>

---

<br/>
<br/>



































# 加密签名

Cryptographic signing: <https://docs.djangoproject.com/zh-hans/2.1/topics/signing/>












# 发送邮件

Sending email: <https://docs.djangoproject.com/zh-hans/2.1/topics/email/>




























<br/>
<br/>

---

<br/>
<br/>











































# 国际化和本地化

i18n: <https://docs.djangoproject.com/zh-hans/2.1/topics/i18n/>




























<br/>
<br/>

---

<br/>
<br/>






























# 日志

Logging: <https://docs.djangoproject.com/zh-hans/2.1/topics/logging/>


Django使用Python内置的logging模块处理系统日志。

























<br/>
<br/>

---

<br/>
<br/>









































# 分页

Pagination: <https://docs.djangoproject.com/zh-hans/2.1/topics/pagination/>


Django提供了一些类来帮助你管理分页数据——跨页分割(Previous/Next)。它们位于`django/core/paginator.py`。


























<br/>
<br/>

---

<br/>
<br/>









































# 安全

Security: <https://docs.djangoproject.com/zh-hans/2.1/topics/security/>


Django的安全功能的概述。








































<br/>
<br/>

---

<br/>
<br/>





































# 性能和优化

本文档概述了一些技术和工具，这些技术和工具可以帮助您更有效地运行Django代码——更快，并且使用更少的系统资源。






































<br/>
<br/>

---

<br/>
<br/>








































# 序列化Django对象

Serialization: <https://docs.djangoproject.com/zh-hans/2.1/topics/serialization/>


Django的序列化框架提供了translating Django Model成其它格式的机制。




























<br/>
<br/>

---

<br/>
<br/>




































# 设置

Settings: <https://docs.djangoproject.com/zh-hans/2.1/topics/settings/>


Django的settings文件包含Django应用的所有配置项。






































<br/>
<br/>

---

<br/>
<br/>

























