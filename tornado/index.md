# Tornado








环境:
- Tornado: v5.1
- Python: v3.6


<br>


参考:

- Docs: <https://www.tornadoweb.org/en/branch5.1/>











<br/>
<br/>

---

<!--more-->

<br/>
<br/>

















# 用户指南

<br/>


## 介绍

Tornado是一个Python Web框架和异步(asynchronous)网络库。通过使用非阻塞(non-blocking)网络I/O，Tornado可以扩展到上万个连接，因此非常适合长轮询(long polling)、WebSocket需要长期连接到的每个用户的应用程序。

Tornado大致可以分为四个主要组件:

- Web框架：包含`RequestHandler`，它的子类用于创建web应用，并支持各种类。
- HTTP的Client和Server的实现：`HTTPServer`和`AsyncHTTPClient`。
- 异步网络库(`IOLoop`和`IOStream`)，用于HTTP组件的构建块，并且还可实现其它协议。
- 协程库(`tornado.gen`)，允许异步代码写的更直接而不用链式回调(chaining callbacks)的方式。

Tornado web框架和HTTP server一起为WSGI提供了一个全栈(full-stack)式选择。为了充分利用Tornado的特性，你需要一起使用Tornado Web框架和HTTP Server。





<br/>
<br/>
<br/>




## 异步和非阻塞I/O

实时(real-time) Web功能需要为每个用户提供一个长时间空闲(mostly-idle)的长连接。在传统的同步(synchronous) web server，这意味着为每个用户提供一个线程(thread)，这是非常昂贵的。
要尽可能减少并发连接(concurrent connections)的开销，Tornado使用一个单线程事件循环。这意味着所有应用程序代码都应该是异步非阻塞的，因为在同一时间只有一个操作是活跃的。
异步和非阻塞这两个术语是非常相关的，并经常交换使用，但它们不是完全相同的事情。

<br/>



### 阻塞

一个函数在等待某些事情的返回的时候会被阻塞(block)。函数阻塞的原因有很多，如：网络IO、磁盘IO、互斥锁...事实上，每个函数在运行和使用CPU的时候或多或少会被阻塞。

一个函数可以在某些方面阻塞，在另外一些方面不阻塞。在Tornado下，我们通常讨论网络IO阻塞，尽管各种阻塞也被最小化。



<br/>
<br/>



### 异步

异步(asynchronous)函数在完成之前返回，在应用中触发下一个动作之前通常会在后台执行一些工作（和正常的同步函数在返回之前就执行完所有的事情不同）。这里列举了几种风格的异步接口：

- 回调参数
- 返回一个占位符
- 传送给一个队列
- 回调注册表





<br/>
<br/>





### 栗子

一个同步(synchronous)函数的栗子:

```
from tornado.httpclient import HTTPClient

def synchronous_fetch(url):
    http_client = HTTPClient()
    response = http_client.fetch(url)
    return response.body
```

一个异步(asynchronous)重写的函数:

```python
from tornado.httpclient import AsyncHTTPClient

async def asynchronous_fetch(url):
    http_client = AsyncHTTPClient()
    response = await http_client.fetch(url)
    return response.body
```

协程(coroutines)有点不可思议，但它们在内如是这样的:

```python
from tornado.concurrent import Future

def async_fetch_manual(url):
    http_client = AsyncHTTPClient()
    my_future = Future()
    fetch_future = http_client.fetch(url)
    def on_fetch(f):
        my_future.set_result(f.result().body)
    fetch_future.add_done_callback(on_fetch)
    return my_future
```

<br>

任何可用协程做的都可传递到回调(callback)对象周围，但协程提供了一个重要的简化让你以相同的方式组织你的代码。这对于错误处理(error handling)尤其重要，在协程预期的`tyr/except`块工作，这是难以实现的回调。





<br>
<br>
<br>







在Tornado中，协程(Coroutines)是推荐的编写异步代码的方式。协程使用Python的`await`或`yield`关键字来暂停(suspend)和恢复(resume)来代替回调链。

协程几乎与同步(synchronous)代码一样简单，但不带线程(thread)的开销。它们使得并发(concurrency)更简单。

栗子:

```python
async def fetch_coroutine(url):
    http_client = AsyncHTTPClient()
	response = await http_client.fetch(url)
	return response.body
```



<br>
<br>



### 原生与装饰的协程

Native vs decorated coroutines

Python 3.5介绍了`async`和`await`关键字。

只要可能，原生协程是推荐的形式。仅需要与旧版本的Python兼容时使用装饰的协程。Tornado文档中一般会使用原生形式。

这两种形式之间的转换一般是简单的:

```python
# Decorated
# Normal function declaration
# with decorator

@gen.coroutine
def a():
    # 'yield' all async funcs
	b = yield c()
	# 'return' and 'yield'
	# cannot be mixed in
	# Python 2, so raise a
	# special execption
	raise gen.Return(b)


# Native
# 'async def' keywords
async def a():
    # 'await' all async funcs
	b = await c()
	# return normally
	return b
```

<br>

两种协程形式的不同:

- 原生协程通常更快
- 原生协程可以使用`async for`和`async with`语句，这使得一些模式更简单
- 除非`await`和`yield`它们，原生协程不会运行所有。装饰的协程一经调用就运行在后台(background)。请注意，这两种协程使用`await`或`yield`都很重要，以便任何异常都有地方可去
- 装饰的协程有与`concurrent.futures`包额外的集成，允许直接yielded `executor.submit`的结果。对于原生协程，使用`IOLoop.run_in_executor`代替
- 通过生成一个列表或字典，装饰的协程支持一些速记。在原生协程中使用`tornado.gen.multi`
- 装饰的协程可以支持与其它软件包的整合。要在原生协程中访问此功能，使用`tornado.gen.convert_yielded`
- 装饰的协程总是返回一个`Future`对象。原生协程返回一个awaitable对象



<br>
<br>





### 如何工作

本节介绍装饰的协程的操作。原生协程在概念上相似，但多了几分复杂。因为与Python runtime额外集成。

包含`yield`的函数是一个生成器(generator)。所有的生成器都是异步的，调用它们时返回一个生成器对象，而不是运行到完成。`@gen.coroutine`装饰器(decorator)通过`yield`表达式与生成器进行通信，通过协程调用返回一个`Future`。

一个协程装饰器的内循环的简单栗子:

```python

# Simplified inner loop of tornado.gen.Runner

def run(self):
    # send(x) makes the current yield return x
    # It returns when the next yield is reached
    future = self.gen.send(self.next)
    def callback(f):
        self.next = f.result()
        self.run()
    future.add_done_callback(callback)
```

装饰器从生成器接收一个`Future`，等待(不会阻塞)选择那些完成的`Future`，解包`Future`并将结果发送回生成器的`yield`表达式。大多数异步代码不直接接触`Future`类，除了由一个异步函数立即传递`Future`到`yield`表达式。





<br>
<br>



### 如何调用协程

协程在正常方式下不抛出异常：它们抛出的任何异常都将在`awaitable`对象直到它被yielded。这意味着以正确的方式调用协程是重要的，或者可能有被你忽略的错误。

```python
async def divide(x, y):
    return x / y


def bad_call():
    # This should raise a ZeroDivisionError, but it won't because
    # the coroutine is called incorrectly.
    divide(1, 0)
```

在几乎所有情况下，调用协程的任何函数都必须是一个协程本身，并在调用中使用`await`和`yield`关键字。当你重写superclass中定义的方法时，查看文档看协程是否被允许。

```python
async def good_call():
    # await will unwrap the object returned by divide() and raise the exception.
    await divide(1, 0)
```

有时，你可能想fire and forget协程，而无需等待其结果。在这种情况下，推荐使用`IOLoop.spawn_callback`，这使得`IOLoop`负责调用。如果失败，`IOLoop`将记录stack trace。

```python
# The IOLoop will catch the exception and print a stack trace in the logs.
# Note that this doesn't look like a normal call, since we pass the function object to be called by the IOLoop.

IOLoop.current().spawn_callback(divide, 1, 0)
```

函数使用`@gen.coroutin`在这种方式下建议使用`IOLoop.spawn_callback`，但它需要函数使用`async def`。

最后，在程序的顶层，如果`IOLoop`尚未运行，就可以启动`IOLoop`，运行协程，然后用`IOLoop.run_sync`方法停止`IOLoop`。这经常用来启动一个面向批处理程序的`main()`函数。

```
# run_sync() doesn't take arguments, so we must wrap the call in lambda.
IOLoop.current().run_sync(lambda: divide(1, 0))
```



<br>
<br>



### 协程模式

Coroutine patterns



#### 调用阻塞函数

Calling blocking functions

从协程调用阻塞函数最简单的方式是使用`IOLoop.run_in_executor`，它返回与协程兼容的`Future`:

```python
async def call_blocking():
    await IOLoop.current().run_in_executor(None, blocking_func, args)
```

<br>

#### Parallelism

`multi`函数接收列表和字典，其值是`Futures`，并等待所有并行(parallel)的`Futures`:

```python
from tornado.gen import multi


async def paraller_fetch(url1, url2):
    resp1, resp2 = await multi([http_client.fetch(url1), http_client.fetch(url2)])

async def paraller_fetch_many(urls):
    responses = await multi (http_client.fetch(url) for url in urls)
    # responses is a list of HTTPResponses in the same order

async def parallel_fetch_dict(urls):
    responses = await multi({url: http_client.fetch(url) for url in urls})
    # responses is a dict {url: HTTPResponse}
```

在装饰的协程，可`yield`列表或字典:



```python
@gen.coroutine
def aprallel_fetch_decorated(url1, url2):
    resp1, resp2 = yield [http_client.fetch(url1), http_client.fetch(url2)]
```

<br>

#### Interleaving

有时保存`Future`是有用的而不立即yielding，因此你可以在等待之前启动其它操作。

```python
from tornado.gen import convert_yielded

async def get(self):
    # convert_yielded() starts the native coroutine in the background.
    # This is equivalent to asyncio.ensure_future() (both work in Tornado).
    fetch_future = convert_yielded(self.fetch_next_chunk())
    while True:
        chunk = yield fetch_future
        if chunk is None: break
        self.write(chunk)
        fetch_future = convert_yielded(self.fetch_next_chunk())
        yield self.flush()
```

这是一个比较容易做装饰的协程，因为它们在调用时立即启动:

```python
@gen.coroutine
def get(self):
    fetch_future = self.fetch_next_chunk()
    if chunk is None: break
    self.write(chunk)
    fetch_future = self.fetch_next_chunk()
    yiield self.flush()
```

<br>

#### Looping

在原生协程，可使用`async for`。在不同版本的Python中，looping is tricky with coroutines，因为没有办法获得对`for`或`while`循环的每次迭代结果的yield。你需要从访问结果分隔循环条件。

```python
import motor

db = motor.MotorClient().test

@gen.coroutine
def loop_example(collection):
    cursor = db.collection.find()
    while (yield cursor.fetch_next):
        doc = cursor.netx_object()
```

<br>

#### 在后台运行

Running in the background

`PeriodicCallback`通常不与协程使用。相反，协程可以包含`While True:`循环并使用`tornado.gen.sleep`:

```python
async def minute_loop():
    while True:
        await do_something()
        await gen.sleep(60)

# Coroutines that loop forever are generally started with spawn_callback().
IOLoop.current().spawn_callback(minute_loop)
```

有时，一个更复杂的循环可能是可取的。例如，前一个循环每`60+N`秒运行，N是`do_something`的运行时间。要准确每60秒运行，使用上面的interleaving模式:

```python
async def minute_loop2()
    while True:
        nxt = gen.sleep(60)  # Start the clock.
        await do_something()  # Run while the clock is ticking.
        await nxt  # Wait for the timer to run out.
```







<br>
<br>
<br>







## Queue

Queue example - a concurrent web spider

Tornado的`tornado.queues`模块实现了协程异步 生产者(producer)/消费者(consumer)模式，类似于由Python标准库的`queue`模块为线程(thread)实现的模式。

yields `Queue.get`协程暂停直到队列中有项。如果队列设置了最大大小集(yield `Queue.put`)协程暂停，直到有另一个项。

`Queue`维护未完成的任务计数，从0开始。`put`递增计数，`task_done`递减它。

web-spider栗子，队列开始仅包含base_url。当worker获取它解析的链接和队列放出新的页面，然后调用`task_done`递减计数。最终，worker取出其url没有过的页面，也没有留在队列中工作。因此，worker调用`task_done`递减计数器归零。主协程，它等待`join`，取消暂停和完成。

```python
import time
from datatime import timedelta
from html.parser import HTMLParser
from ulllib.parse import urljoin, urldefrag

from tornado import gen, httpclient, ioloop, queues

base_url = 'http://www.tornadoweb.org/en/stable/'
concurrency = 10



async def get_links_from_url(url):
    """Download the page at `url` and parse it for links.
    Returned links have had the fragment after `#` removed, and have been made bsolute so,
    e.g. the URL 'gen.html#tornado.gen.coroutine' becomes
    'http://www.tornadoweb.org/en/stable/gen.html'.
    """

    response = await httpclient.AsyncHTTPClient().fetch(url)
    print('fetched %s' % url)

    html = response.body.decode('errors='ignore')
    return [urljoin(url, remove_fragment(new_url))
            for new_url in get_links(html)]

def remove_fragment(url):
    pure_url, frag = urldefrag(url)
    return pure_url



def get_links(html):
    class URLSeeker(HTMLParser):
        def __init__(self):
            HTMLParser.__init__(self)
            self.urls = []

        sef handle_starttag(self, tag, attrs):
            href = dict(attrs).get('href')
            if href and tag == 'a':
                self.urls.append(href)

    url_seeker = URLSeeker()
    url_seeker.feed(html)
    return url_seeker.urls



async def main():
    q = queues.Queue()
    start = time.time()
    fetching, fetched = set(), set()

    async def fetch_url(currrent_url):
        if current_url in fetching:
            return

        print('fetching %s' % current_url)
        fetching.add(current_url)
        urls = await get_links_from_url(current_url)
        fetched.add(current_url)

        for new_url in urls:
            # Only follow links beneath the base URL
            if new_url.startswith(base_url):
                await q.put(new_url)

    async def worker():
        async for url in q:
            if url in None:
                return
            try:
                await fetch_url(url)
            except Exception as e:
                print('Exception: %s %s' % (e, url))
            finally:
                q.task_done()

    await q.put(base_url)

    # Start workers, then wait for the work queue to be empty.
    workers = gen.multi([worker() for _ in range(concurrency)])
    await q.join(timeout=timedelta(seconds=300))
    assert fetching == fetched
    print('Done in %d seconds, fetched %s URLs.' % (
        time.time() - start, len(fetched)))

    # Signal all the workers to exit.
    for _ in range(concurrency):
        await q.put(None)
    await workers



if __name__ == '__main__':
    io_loop = ioloop.IOLoop.current()
    io_loop.run_sync(main)
```







<br>
<br>
<br>








## Tornado web程序结构

Structure of a Tornado web application



一个Tornado web程序通常由一个或多个`RequestHandler`子类组成，`Application`对象是哪些路由进入的请求的处理程序(handler)，`main()`函数来启动server。

一个最小化的hello world栗子:

```python
import tornado.ioloop
import tornado.web

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write("Hello, world")

def make_app():
    return tornado.web.Application([
        (r"/", MainHandler),
    ])

if __name__ == "__main__":
    app = make_app()
    app.listen(8888)
    tornado.ioloop.IOLoop.current().start()
```



<br>



### Application对象

`Application`对象是负责全局配置，包括映射请求到处理程序(handler)的路由表。

路由表是`URLSpec`对象的列表(或元组)，其中每一个包含(至少)一个正则表达式和一个处理类(handler class)。顺序匹配，第一匹配规则被使用。如果正则表达式中包含捕获组，这些组的路径参数将被传递给处理程序(handler)的HTTP方法。如果字典作为`URLSpec`的第三个参数传递，它提供将初始化参数传递给`RequestHandler.initialize`。最后，`URLSpec`可以有一个名称，这将允许它与`RequestHandler.reverse_url`使用。

栗子:

```python
# / URL 映射到 MainHandler
# /story/后跟数字 映射到 StoryHandler，数字(作为字符串)被传递给StoryHandler.get

class MainHandler(RequestHandler):
    def get(self):
        self.write('<a href="%s"> link to story 1</a>' %
                  self.reverse_url("story", "1"))

class StoryHandler(RequestHandler):
    def initialize(self, db):
        self.db = db

    def get(self, story_id):
        self.write("this is story %s" % story_id)

app = Application([
    url(r"/", MainHandler),
    url(r"/story/([0-9]+)", StoryHandler, dict(db=db), name="story")
])
```

`Application`构造器采用许多关键字参数，可用于定制应用程序的行为和启用可选功能。查看`Application.settings`获取完整列表。



<br>
<br>



### RequestHandler子类

Subclassing RequestHandler

大部分Tornado Web应用程序的工作是`RequestHandler`子类完成的。主入口点的处理程序子类(handler subclass)是正在处理的HTTP方法(`get()`, `post()`)的方法命名。例如，每个handler可以定义这些方法中的一种或多种，以处理不同的HTTP动作。如上所述，这些方法将于对应于匹配的路由规则的捕获组参数来调用。

在处理程序内部，调用如`RequestHandler.render`或`RequestHandler.write`来产生响应(response)。`render()`通过名称加载一个模板，并与给定的参数来渲染它。`write()`被用于非基于模板(non-template-based)输出。它接受字符串，字节和字典(字典被编码为json)。

`ReqestHandler`中的许多方法都设计在子类中重写(overridden)，并在整个application中使用。这是常见的定义`BaseHandler`类，覆盖方法如`write_error`, `get_current_user`，并为你所有指定的handler继承`BaseHandler`而不是`RequestHandler`。



<br>
<br>



### 处理请求输入

Handling request input

request handler可以访问表示与`self.quest`获取当前请求的对象。查看`HTTPServerRequest`类来获取完整的列表。

通过HTML表单中使用的格式请求的数据将为你解析，并在如`get_query_argument`和`get_body_argument`方法中可用。

```python
class MyFormHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('<html><body><form action="/myform" method="POST">'
                   '<input type="text" name="message">'
                   '<input type="submit" value="Submit">'
                   '</form></body></html>')

    def post(self):
        self.set_header("Conten-Type", "text/plain")
        self.write("You wrote" + self.get_body_argument)
```

由于HTML表单的编码是模糊的，以元素中的一个参数是否为单一值(single value)或一个列表，`RequestHandler`有独特的方法，以允许application表明它是否期望一个列表。对于列表，使用`get_query_arguments`和`get_body_arguments`来代替它们的singular counterparts。

通过表单上传的文件在`self.request.files`可用，它映射名称(html `input type="file"`元素)到一个文件列表。每个文件是`{"filename":..., "content_type":..., "body":...}`格式的字典。`files`对象仅表示文件是否以一种form wrapper上传(如`multipart/form-data` 内容类型)。如果不使用这种格式，原始上传数据在`self.request.body`可用。默认情况下上传的文件在内存中完全缓冲(fully buffered)。如果你要处理的文件太大，但想在内存中舒适保存，可参考`stream_request_body`类装饰器。

由于HTML格式编码的怪癖，Tornado并不试图统一参数和其它输入类型的形式。特别是，我们不解析JSON请求主体。Applicaton希望使用JSON而不是form-encoding可以覆盖`prepare`来解析它们的请求:

```python
def prepare(self):
    if self.request.headers.get("Content-Type", "").startswith('"application/json"'):
        self.json_args = json.loads(self.request.body)
    else:
        self.json_args = None
```



<br>
<br>



### 重写RequestHandler方法

Overriding RequestHandler methods

除了`get()`, `post()`...，在`RequestHandler`某些其它方法被设计成在必要时由子类重写。在每次请求，调用以下顺序进行:

1. 在每个请求上，一个新的`RequestHandler`对象被创建
2. `initialize()`被调用，从Application配置的初始化参数。初始化通常应该只保存传入成员变量的参数，不产生任何输出或调用(如`send_error`)
3. `prepare()`被调用。这是最有用的，由所有handler subclass共享的基类，作为prepare被无论哪个HTTP方法所调用。`prepare`可产生输出，如果它调用`finish`或`redirect`，这里处理停止
4. 当其中一个HTTP方法被调用时: `get()`, `post()`, `put()`。如果URL正则中包含捕获组(capturing group)，它们将被作为参数传递给该方法
5. 当请求完成后，调用`on_finish()`。对于大多数handler这个在`get()`返回后立即调用。在调用`finish()`之后使用`tornado.web.asynchronous`装饰器来装饰handler

在`RequestHandler`文档中，所有的方法都设计来可重写。一些最常用的重写方法:

- `writre_error`: 输出HTML错误页面
- `on_connection_close`: 当客户端断开连接时调用。应用可选择检测此情况并停止进一步的处理。注意，不能保证一个关闭的连接能够被及时发现
- `get_current_user`
- `get_user_locale`: 返回当前用户的Locale对象
- `set_default_headers`: 用于在响应中设置其它header



<br>
<br>



### 错误处理

Error Handling

如果handler抛出一个异常，Tornado将调用`RequestHandler.write_error`来生成一个错误页。`tornado.web.HTTPError`可用来生成一个特定状态码。所有其它异常返回500状态。

debug模式下的默认错误页面包含一个stack trace和对错误的一行说明。要生成自定义错误页，重写`RequestHandler.write_error`。可通过如`write`和`render`方法来产生输出。如果错误是由异常导致的，一个`exc_info`将作为一个关键字参数传递。

也可通过调用`set_status`产生与常规处理方法`write_error`生成的错误页面，编写一个响应，并返回。特殊异常`tornado.web.Finish`可抛出终止处理而不调用`write_error`在简单返回不方便时。

对于404错误，使用`default_handler_class`应用设置(Application setting)。此处理程序应重写`prepare`，而不是像`get()`方法更具体的方法，所以它与任何HTTP方法工作。如上所述应该产生错误页面: 要么抛出`HTTPError(404)`和重写`write_error`，或调用`self.set_status(404)`和直接在`prepare()`中产生响应。



<br>
<br>



### 重定向

Redirection

Tornado中有两种主要的方式重定向请求: `RequestHandler.redirect`和`RedirectHandler`。

可在`RequestHandler`方法中使用`self.redirect()`来重定向到别处。这有一个`permanent`的可选参数，可用它来表示永久的重定向。`permanent`的默认值为`False`，其产生一个`302 Found` HTTP响应码，适合像`POST`请求成功之后使用。如果`permanent`为`true`， 则使用`301 Moved Permanently` HTTP响应码，其用于重定向到一个规范友好的URL。

`RedirectHandler`让你直接在Application路由表中配置重定向，栗子:

```python
app = tornado.web.Application([
    url(r"/app", tornado.web.RedirectHandler,
        dict(url="http://xxx.com")),
])
```

`RedirectHandler`同样支持正则表达式取代。栗子:

```python
app = tornado.web.Application([
    url(r"/photos/(.*)", MyPhotoHandler),
    url(r"/pictures/(.*)", tornado.web.RedirectHandler,
        dict(url=r"/photo/{0})),
])
```

不像`RequestHandler.redirect`，`RedirectHandler`默认使用永久重定向。这因为路由表在运行时不发生变化，被认定为时永久性的，而在处理中发现重定向可能改变其它逻辑的结果。要使用`RedirectHandler`发送一个临时的重定向，将`permanent=False`添加到`RedirectHandler`初始化参数。



<br>
<br>



### 异步处理程序

Asynchronous handlers

某些处理方法(如`prepare()`和HTTP的`get()`, `post()`...)可能会被重写为协程，使处理程序异步。

Tornado同样支持使用`tornado.web.asynchronous`装饰器异步处理的回调风格，但这种风格已经过时，将在Tornado6中一处。新的应用应该使用协程来代替它。

使用协程的一个简单处理程序的栗子:

```python
class MainHandler(tornado.web.RequestHandler):
    async def get(self):
        http = tornado.httpclient.AsyncHTTPClient()
        response = await http.fetch("http://friendfeed-api.com/v2/feed/bret")
        json = tornado.escape.json_decode(response.body)
        self.write("Fetched " + str(len(json["entries"])) + " entries "
                   "from the FriendFeed API")
```

更多高级的异步的栗子，查考文档。







<br>
<br>
<br>







## 模板和UI

Templates and UI

Tornado包含了一个简单、快速、灵活的模板语言。想想Django和Jinja2。

Tornado还可与任何其它Python模板语言使用，虽然没有规定集成这些系统到`RequestHandler.render`里。简单地渲染模板为字符串，并将其传递到`RequestHandler.write`。



<br>



### 配置模板

Configuring templates

默认情况下，Tornado在引用它的`.py`文件中的同一目录下查找模板文件。要把模板文件放在不同的目录中，使用`template_path`应用设置。如果你有不同的模板路径用于不同的处理程序，请重写`RequestHandler.get_template_path`。

要从非文件系统位置载入模板，子类`tornado.template.BaseLoader`将在模板并传递一个实例作为`template_loader`应用设置。

默认缓存编译的模板。要关闭这个缓存和重新加载模板，使用`compiled_template_cache=False`或`debug=True`应用设置。



<br>
<br>



### 模板语法

Template syntax

Tornado模板仅仅是HTML(或其它基于文本的格式)与Python控制序列和嵌入在标记内的表达式，想想Django模板和Jinja2。

表达式可以是任意Python表达式，包括函数调用。模板代码在包括以下对象和函数的命名空间执行(请注意，以下列表适用于使用`RequestHandler.render`和`render_string`渲染模板。如果你直接使用在`RequestHandler`外的`tornado.template`模块，那么许多内容是不存在的。)

- `escape`: `tornado.escape.xhtml_escape`的别名
- `xhtml_escape`: `tornado.escape.xhtml_escape`的别名
- `url_escape`: `tornado.escape.url_escape`的别名
- `json_encode`: `tornado.escape.json_encode`的别名
- `squeeze`: `tornado.escape.squeeze`的别名
- `linkify`: `tornado.escape.linkify`的别名
- `datetime`: Python的`datetime`模块
- `handler`: 目前的`RequestHandler`对象
- `request`: `handler.request`的别名
- `current_user`: `handler.current_user`的别名
- `locale`: `handler.locale`的别名
- `_`: `handler.locale.translate`的别名
- `static_url`: `handler.static_url`的别名
- `xsrf_form_html`: `handler.xsrf_form_html`的别名
- `reverse_url`: `Application.reverse_url`的别名
- 所有条目从应用的`ui_methods`和`ui_modules`
- 任何关键字参数传递给`render`或`render_string`

当你在构建一个真正的应用时，你会想要使用Tornado模板的所用功能，尤其是模板继承。阅读`tornado.template`部分了解详细信息。

引擎盖下，Tornado模板直接转换为Python。模板中的表达式是逐字复制到Python函数中。我们不设法防止模板语言的任何东西。最后，如果你写的模板表达式内随机的东西，当你执行模板可能会获得随机的Python错误。

所有的模板输出默认被转义(escape)，使用`tornado.escape.xhtml_escape`函数。这个行为可通过全局地传递`autoescape=None`给应用或`tornado.template.Loader`构造器，对于模板文件指示```{% autoescape None%}```或通过```{% raw ... %}```代替```{{ ... }}```。此外，在每一个可选择转义函数名的地方，可用`None`代替。

虽然Tornado的自动转义为避免XSS漏洞有帮助，但它并不是在所有情况下都有效。例如在JS或CSS表达式的某些地方，可能需要额外的转义。此外，必须小心地使用HTML双引号`"`和`xhtml_escape`，可能包含不受信任的内容，或者必须为属性使用单独地转义函数。



<br>
<br>



### UI模块

UI modules

Tornado支持UI模块，可以很容易地在你的应用中支持标准的、可重用的UI组件。UI模块都喜欢特殊的函数调用来渲染网页和组件，它们可以包装自己的CSS和JS。

例如，如果要实现一个博客，你想拥有的博客条目同时出现在博客主页和每个博客页面上，你可以编写一个`Entry`模块在两个页面上渲染它们。首先，为你的UI模块创建一个Python模块:

```py
# uimodules.py
class Entry(tonado.web.UIModule):
    def render(self, entry, show_commnets=False):
        return self.render_string("module-entry.html, entry=entry, show_comments=show_comments")
```

在应用中设置`ui_modules`告诉Tornado使用`uimodules.py`:

```py
from . import uimodules

class HomeHandler(tornado.web.RequestHandler):
    def get(self):
        entries = self.db.query("SELECT * FROM entries ORDER BY date DESC")
        self.render("home.html", entries=entries)

class EntryHander(tornado.web.ReqestHandler):
    def get(self, entry_id):
        entry = self.db.get("SELECT * FROM entries WHERE id = %s, entry_id")
        if not entry: raise tornado.web.HTTPError(404)
        self.render("entry.html", entry=entry)


settings = {
    "ui_modules": uimodules,
}

application = tornado.web.Application([
    (r"/", HomeHandler),
    (r"/entry/([0-9])", EntryHandler),
], **settings)
```

在模板内，你可以使用```{% module %}```调用模块，例如在`home.html`中调用`Entry`模块:

```jinja2
{% for entry in entries%}
{% module Entry(entry) %}
{% end %}
```

`entry.html`中:

```
{% module Entry(entry, show_comments=True) %}
```

模块可以通过重写`embedded_css`, `embedded_javascript`, `javascript_files`或`css_files`方法来包含自定义的CSS和JS函数:

```python
class Entry(tornado.web.UIModule):
    def embedded_css(self):
        return ".entry { margin-bottom: 1em; }"

    def render(self, entry, show_comments=False):
        return self.render_string("module-entry", show_comments=show_comments)
```

模块CSS和JS将包含一次，不管一个页面中这个模块使用了多少次。CSS总是包含在页面的`<head>`，JS总是包含在`</body>`标记之前在页面的页面结束标记。

当不需要附加的Python代码，模板文件本身可以用作一个模块。例如，前面的栗子可以改写在`module-entry.html`模块:

```
{{ set_resources(embedded_css=".entry { margin-bottom: 1em; }") }}
<!-- more template html... -->
```

经修订的模板模块将与下栗被调用:

```
{% module Template("module-entry.html", show_comments=True) %}
```

该`set_resources`功能尽在通过
```{% module Template(...) %}```
调用模板。不同于
```{% include %}```，
模板模块具有从它们的包含模板的独特命名空间——它们只能看到全局模板命名空间和自己的关键字参数。






<br>
<br>
<br>







## 认证和安全

Authentication and security


<br>


### Cookie和secure cookies

可以使用`set_cookie`方法在用户浏览器中设置cookie:

```python
class MainHandler(tornado.web.ReqestHandler):
    def get(self):
        if not self.get_cookie("mycookie"):
            self.set_cookie("mycookie", "myvalue")
            self.write("Your cookie was not set yet!")
        else:
            self.write("Your cookie was set!")
```

cookie是不安全的，可以很容易地被客户修改。如果你需要设置cookie，请确定当前登录的用户，你需要签属(signed)你的cookie来防止伪造。Tornado支持使用`set_secure_cookie`和`get_secure_cookie`方法来签属(sign)cookie。要使用这些方法，你需要在创建应用时指定一个名为`cookie_secret`的密钥键。你可以在应用中设置关键字参数来传递给应用。

```python
application = tornado.web.Application([
    (r"/", MainHandler),
], cookie_secret="__TODO:_GENERATE_YOUR_OWN_RANDOM_VALUE_HERE__")
```

签属的cookie含有时间戳和HMAC签名的cookie编码值。如果cookie是旧的，或者签名不匹配，`get_secure_cookie`将会返回`None`就像没有设置cookie那样。上面栗子的安全版本:

```python
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        if not self.get_secure_cookie("mycookie"):
            self.set_secure_cookie("mycookie", "myvalue")
            self.write("Your cookie was not set yet!")
        else:
            self.write("Your cookie was set!")
```

Tornado的secure cookie保证完整性，但不保密。也就是说，cookie不能被修改，但可以被用户看到。`cookie_secret`是一个对称密钥并且必须保密——得到这个值的人都可以制作自己的签名的cookie。

默认情况下，Tornado的cookie在30天后过期。可对`set_secure_cookie`使用`expires_days`参数和`max_age_days`来修改。

Tornado同样支持多个签名密钥来启用签名轮询。`cookie_secret`必须与整数密钥版本作为关键字和相应的secret作为字典的值。将当前使用的签名密钥必须在应用中设置为`key_version`，但在字典的所有其它键都允许cookie签名认证，如果设置在cookie中的是正确的密钥版本。要更新cookie，可通过查询`get_secure_cookie_key_version`获取当前的签名密钥版本。



<br>
<br>



### 用户认证

User authentication

当前已认证的用户在每个request handler中可使用`self.current_user`，在每个模板中为`current_user`。默认情况下，`current_user`为`None`。

要在应用中执行用户身份认证，需要在request handler中重写`get_current_user`以基于cookie的值确定当前用户。下面是一个让用户登录到应用，简单地指定一个昵称，然后将其保存到cookie中:

```python
class BaseHandler(tornado.web.RequestHandler):
    def get_current_user(self):
        return self.get_secure_cookie("user")

class MainHandler(BaseHandler):
    def get(self):
        if not self.current_user:
            self.redirect("/login")
            return
        name = tornado.escape.xhtml_escape(self.current_user)
        self.write("Hello, " + name)

class LoginHandler(BaseHandler):
    def get(self):
        self.write('<html><body><form action="/login" method="post">'
            'Name: <input type="text" name="name">'
            '<input type="submit" value="Sign in">'
            '</form></body></html>')

    def post(self):
        self.set_secure_cookie("user", self.get_argument("name"))
        self.redirect("/")

application = tornado.web.Application([
    (r"/", MainHandler),
    (r"/login", LoginHandler),
], cookie_secret="__TODO:_GENERATE_YOUR_OWN_RANDOM_VALUE_HERE__")
```

你可以要求用户在使用`tornado.web.authenticated` Python装饰器处登录。如果请求的方法带有此装饰器，并且用户没有登录，则他们将被重定向到`login_url`或其它设置。重写上面的栗子:

```python
class MainHandler(BaseHandler):
    @tornado.web.authenticated
    def get(self):
        name = tornado.escaple.xhtml_escape(self.current_user)
        self.write("Hello, " + name)

settings = {
    "cookie_secret": "__TODO:_GENERATE_YOUR_OWN_RANDOM_VALUE_HERE__",
    "login_url":"/login",
}

application = tornado.web.Application([
    (r"/", MainHandler),
    (r"/login", LoginHandler),
], **settings)
```



如果你使用`authenticate`装饰器装饰一个`post()`方法，并且用户没有登录，则Server会返回403响应。`@authenticated`装饰器简单来说就是`if not self.current_user: self.redirect()`的快捷键，并且可能不适用于非基于浏览器的登录方案。



<br>
<br>



### 第三方认证

Third party authentication

`tornado.auth`模块实现了许多受欢迎的网站上提供的认证(authentication)和授权(authorization)协议，包括Google, FaceBook, Twitter...

下面是一个使用谷歌认证的示例，存储Google credential到cookie以便后续访问使用:

```python
class GoogleOAuth2LoginHandler(tornado.web.RequestHandler, tornado.auth.GoogleOAuth2Mixin):
    async def get(self):
        if self.get_argument('code', False):
            user = await self.get_authenticated_user(
                redirect_uri="http://your.site.com/auth/google",
                code=self.get_argument('code'))
            # Save the user with e.g. set_secure_cookie
        else:
            await self.authorize_redirect(
            redirect_uri='http://your.site.com/auth/google',
            client_id=self.setting['google_oauth']['key'],
            scope=['profile', 'email'],
            response_type='code',
            extra_params={'approval_prompt': 'auto'})
```

更多详细内容，请参考`tornado.auth`文档。



<br>
<br>



### 跨站请求伪造保护

Cross-site request forgery protection

跨站请求伪造(Cross-site request forgery, XSRF)，是Web应用的一个常见的问题。

防止XSRF普遍接受的解决方案是每个用户的cookie使用不可预测的值，此值包含网站上每个表单提交的额外参数。如果表单提交的cookie和值不匹配，则请求可能是伪造的。

Tornado内置了XSRF保护。要在你的站点中包含它，启用应用`scrf_cookies`设置:

```python
settings = {
    "cookie_secret": "__TODO:_GENERATE_YOUR_OWN_RANDOM_VALUE_HERE__",
    "login_url": "/login",
    "xsrf_cookies": True
}
application = tornado.web.Application([
    (r"/", MainHandler),
    (r"/login", LoginHandler),
], **settings)
```

如果设置了`xsrf_cookies`，Tornado Web Application将为所有用户设置`_xsrf` cookie，并拒绝没有包含正确的`_xsrf`值的所有`POST`, `PUT`, `DELETE`请求。如果你打开了此设置，你需要一切形式的`POST`提交中包含此字段。你可以使用特殊的UIModule `vsrf_form_html()`，在所有模板中可用:

```jinja2
<form action="/new_message" method="post">
  {% module xsrf_form_html() %}
  <input type="text" name="message"/>
  <input type="submit" value="Post"/>
</form>
```

如果你提交AJAX `POST`请求，你还需要构造JS来包括每个请求的`_xsfr`值。所有包含`_xsrf`请求AJAX POST的JQuery函数:

```jquery
function getCookie(name) {
    var r = document.cookie.match("\\b" + name + "=([^;]*)\\b");
    return r ? r[1] : undefined;
}

jQuery.postJSON = function(url, args, callback) {
    args._xsrf = getCookie("_xsrf");
    $.ajax({url: url, data: $.param(args), dataType: "text", type: "POST",
        success: function(response) {
        callback(eval("(" + response + ")"));
    }});
};
```

对于`PUT`和`DELETE`请求，XSRF token可能会通过HTTP `X-XSRFToken` Header进行传递。使用`xsrf_form_html`时，XSRF cookie被正常设置，但是在不使用任何形式的纯JS应用中，可能需要手动访问`self.xsrf_token`。

如果你需要在每个handler中自定义XSRF行为，你可以重写`RequestHandler.check_xsrf_cookie()`。例如，如果你有一个不使用cookie的API，你可能希望通过使`check_xsrf_cookie`什么也不做来禁用XSRF保护。然而，如果你支持基于cookie和非基于cookie的认证，只要求当前请求使用cookie认证XSRF保护是重要的。



<br>
<br>



### DNS重新绑定

DNS Rebinding

DNS重新绑定是一种攻击，可以绕过同源策略，并允许外部站点访问内部网络的资源。使用TLS的应用不容易受到这种攻击。没有使用TLS的应用依赖网络层的访问控制，应警惕通过验证的HTTP Header的`Host`被DNS重新绑定。This means passing a restrictive hostname pattern to either a `HostMatches` router or the first argument of `Application.add_handlers`:

```python
# BAD: uses a default host pattern of r'.*'
app = Application([('/foo', FooHandler)])

# GOOD: only matches localhost or its ip address.
app = Application()
app.add_handlers(r'(localhost|127\.0\.0\.1)',
                 [('/foo', FooHandler)])

# GOOD: same as previous example using tornado.routing.
app = Application([
    (HostMatches(r'(localhost|127\.0\.0\.1)'),
            [('/foo', FooHandler)]),
    ])
```

此外，应用的`default_host`参数，和`DefaultHostMatches`路由器不能在应用中使用，这可能受到DNS重新绑定，因为它有一个通配符主模式类似的效果。



<br>
<br>
<br>



## 运行和部署

Running and deploying

自从Tornado提供了自己的HTTPServer，运行和部署它便和其它Python Web框架有点不同。不同于配置WSGI，你只需要写一个`main()`函数来启动Server:

```python
def main():
    app = make_app()
    app.listen(8888)
    IOLoop.current().start()

if __name__ == '__main__':
    main()
```

请注意，这可能需要增加每个进程可打开的文件数(open files)，可能修改`ulimit`限制。

<br>
<br>



### 进程和端口

Processes and ports

由于Python的GIL(Global Interpreter Lock)，有必要运行多个Python进程，以充分利用多CPU机器。通常，最好为每个CPU运行一个进程。

Tornado包含了一个内置的多进程模式，一次可启动多个进程。这需要稍微修改以下启动方式:

```python
def main():
    app = make_app()
    server = tornado.httpserver.HTTPServer(app)
    server.bind(8888)
    server.start(0)  # forks one process per cpu
    IOLoop.current().start()
```

这是启动多个进程，并让它们使用相同的端口最简单的方法，虽然它有一定的局限性。首先，每个子进程都会有自己的IOLoop，因此在fork前没有事物触及IOLoop示例是很重要的。第二，在这个模型中很难做到零停机更新(zero-downtime updates)。最后，由于所有的进程共享同一端口更难以单独监控。

对于更复杂的部署，建议单独启动进程，并监听不同的端口。`supervisord`是一个好办法。当每个进程使用了不同的端口，通常需要一个外部的负载均衡器(如HAProxy, Nginx)以单独的访问地址提供给访问者。



<br>
<br>



### 运行在负载均衡器后

Running behind a load balancer

当运行在如Nginx这样的负载均衡器之后，建议传递`xheaders=True`给HTTPServer构造器。这将告诉Tornado使用`X-Real-IP`用户Header，来获取用户IP地址，而不是负载均衡器的IP地址。

一个栗子:

```conf
user nginx;
worker_processes 1;

error_log /var/log/nginx/error.log;
pid /var/run/nginx.pid;

events {
    worker_connections 1024;
    use epoll;
}

http {
    # Enumerate all the Tornado servers here
    upstream frontends {
        server 127.0.0.1:8000;
        server 127.0.0.1:8001;
        server 127.0.0.1:8002;
        server 127.0.0.1:8003;
    }

    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    access_log /var/log/nginx/access.log;

    keepalive_timeout 65;
    proxy_read_timeout 200;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    gzip on;
    gzip_min_length 1000;
    gzip_proxied any;
    gzip_types text/plain text/html text/css text/xml
               application/x-javascript application/xml
               application/atom+xml text/javascript;

    # Only retry if there was a communication error, not a timeout
    # on the Tornado server (to avoid propagating "queries of death"
    # to all frontends)
    proxy_next_upstream error;

    server {
        listen 80;

        # Allow file uploads
        client_max_body_size 50M;

        location ^~ /static/ {
            root /var/www;
            if ($query_string) {
                expires max;
            }
        }
        location = /favicon.ico {
            rewrite (.*) /static/favicon.ico;
        }
        location = /robots.txt {
            rewrite (.*) /static/robots.txt;
        }

        location / {
            proxy_pass_header Server;
            proxy_set_header Host $http_host;
            proxy_redirect off;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Scheme $scheme;
            proxy_pass http://frontends;
        }
    }
}
```



<br>
<br>



### 静态文件和侵略性的文件缓存

Static files and aggressive file caching

你可以通过在应用中指定`static_path`来设置Tornaodo提供静态文件:

```py
settings = {
    "static_path": os.path.join(os.path.dirname(__file__), "static"),
    "cookie_secret": "__TODO:_GENERATE_YOUR_OWN_RANDOM_VALUE_HERE__",
    "login_url": "/login",
    "xsrf_cookies": True,
}

application = tornado.web.Application([
    (r"/", MainHandler),
    (r"/login", LoginHandler),
    (r"/(apple-touch-icon\.png)", tornado.web.StaticFileHandler,
     dict(path=settings['static_path'])),
], **settings)
```

此设置会自动设置以`/static/`的所有请求到静态目录，如`http://localhost:8888/static/foo.png`将从指定的静态目录提供静态文件。同样还有`/robots.txt`和`/favicon.ico`，即便它们并未以`/static/`为前缀。

在上面的设置，我们已明确的配置Tornado从`StaticFileHandler`提供`apple-touch-icon.png`。

要提高性能，通常是浏览器缓存静态资源，因此浏览器将不会发送不必要的`If-Modified-Since`或`Etag`请求，这可能会阻止页面的渲染。Tornado支持这一开箱即用的静态内容版本。

要使用此功能，在你的模板中使用`static_url`方法，而不是在你的HTML中直接输入静态文件:

```html
<html>
   <head>
      <title>FriendFeed - {{ _("Home") }}</title>
   </head>
   <body>
     <div><img src="{{ static_url("images/logo.png") }}"/></div>
   </body>
 </html>
```



`static_url()`函数会将相对路径转换为如`/static/images/logo.png?v=aae54`这样的URI。`v`参数是`logo.png`的哈希内容，它的存在使得Tornado Server发送cache header到用户浏览器，这将使浏览器无限期缓存内容。

由于`v`参数使基于文件的内容，如果你更新文件并重启Server，它将发送一个新的`v`值，因此用户浏览器会自动获取新的文件。如果文件的内容没有改变，浏览器将继续使用本地缓存的副本而没有检查Server上的更新，显著提供渲染性能。

在生产环境，你可能希望从像Nginx这样更优化的静态文件服务器提供静态文件。你可以配置几乎所有的Web Server识别由`static_url`使用的标签，并设置相应的cache header。

栗子:

```conf
location /static/ {
    root /var/friendfeed/static;
    if ($query_string) {
        expires max;
    }
 }
```



<br>
<br>



### Debug模式和自动重载

Debug mode and automatic reloading

如果将`debug=True`传递给`Application`构造器，应用将运行在debug/development模式下。在此模式下，一些便于开发调试的功能将被启用:

- `autoreload=True`：应用会监视更改的源文件并在发生变化时自动重载。这样减少了在开发过程中手动重启服务。然后，某些错误可能导致无法启动。
- `compiled_template_cache=False`：模板不会被缓存。
- `static_hash_cache=False`：静态文件哈希值(由`static_url`函数使用)将不会被缓存。
- `serve_traceback=True`：当RequestHandler中的异常没有被捕获，将会生成一个包含stack trace的错误页面。

自动重载模式不兼容`HTTPserver`的多进程模式。如果你正在使用自动重载模式，你不要给`HTTPServer.start`一个或多于一个参数(或调用`tornado.process.fork_processes`)。

调式模式的自动重载功能是可用作为`tornado.autoreload`独立(standalone)模块。这两个可以组合使用，以提供对语法错误的额外稳健：在应用中设置`autoreload=True`来在运行时检测改变，并使用`python -m tornado.autoreload myserver.py`启动来在启动时捕获任意语法错误或其它错误。

重载将失去任何Python解释器命令行参数(如`-u`)，因为它使用`sys.executable`和`sys.argv`来重新执行Python。此外，修改这些变量将导致重载行为不正确。



<br>
<br>


### WSGI

Tornado通常是为了独立运行，而不用WSGI容器。然而，在一些环境中（如Google App Engine），只允许WSGI，应用程序无法运行自己的Server。在这种情况下，Tornado支持操作的限制模式，不支持异步操作，但允许在只有WSGI环境的Tornado功能的子集。未在WSIG模式允许的功能包括协程、`@asynchronous`装饰器，`AsyncHTTPclient`、`auth`模块和WebSockets。

你可以使用`tornado.wsgi.WSGIAdapter`将一个Tornado Application转换为WSGI application。

栗子:

```python
import tornado.web
import tornado.wsgi

class MainHandler(tornado.web.RequestHandler):
    def get(self):
        self.write('Hello, world')

tornado_app = tornado.web.Application([
    (r"/", MainHandler),
])

application = tornado.wsgi.WSGIAdapter(tornado_app)
```


