---
title: HTTP请求方法和状态码
date: 2017-12-01 13:53:20
tags: "HTTP"
categories: "Network"

---

# 常见HTTP请求方法


HTTP协议的请求方法有：`GET, POST, HEAD PUT DELETE, OPTIONS, TRACE, CONNECT`

<!--more-->

Method | Description
- | -
GET | 向Server请求文件
POST | 向Server发送数据并让Server进行处理
PUT | 向Server发送数据并存储在Server端
HEAD | 检查一个对象是否存在
DELETE | 从Server上删除一个文件
CONNECT | 对通道提供支持
TRACE | 跟踪到Server的路径
OPTION | 查询Server的性能






<br>
<br>

---

<br>


# HTTP Status Code

当我们从Client向Server发送请求时，Server会向我们返回StatusCode。
StatusCode会告诉我们Server的响应的状态，通过它，我们就可以知道当前请求是成功还是出现了问题。

HTTP StatusCode放置在HTTP  Response报文中。

StatusCode由三位数字组成，第一个数字定义了响应类型，有五种可能值：

状态码 | 响应类别 | 描述
- | - | -
1xx | 指示信息 | 服务器正在处理请求
2xx | 成功 | 请求以正常处理完毕
3xx | 重定向 | 需要进行额外操作以完成请求
4xx | 客户端错误 | 客户端原因导致服务器无法处理请求
5xx | 服务器错误 | 服务器原因导致处理请求出错



<br>
<br/>


## 常见HTTP状态码

状态码 | 描述
- | -
200-OK | 服务器成功返回网页，这是成功的HTTP请求返回的标准状态码
301 - Moved Permanently | 永久跳转，所有请求的网页将永久跳转到被设定的新位置
400 - Bad Request | 客户端请求有语法错误，不能被服务器理解
403 - Forbidden | 禁止访问，这个请求时合法的，但是服务器端因为匹配了预先设置的规则而拒绝响应客户端的请求，此类问题一般为服务器权限配置不当所致
404 - Not Found | 服务器找不到客户端请求的指定页面，可能是客户端请求了服务器不存在的资源所导致
500 - Internal Server Error | 内部服务器错误，服务器遇到了意料不到的情况，不能完成客户的请求。这是一个较为笼统的报错，一般为服务器的设置或内部程序问题所致
502 - Bad Gateway | 坏的网关，一般是代理服务器请求后端服务器时，后端服务不可用或没有完成响应网关服务器。一般为代理服务器下面的节点出了问题
503 - Service Unavailable | 服务当前不可用，可能为服务器超载或停机维护所致，或者是代理服务器后面没有可以提供服务的节点
504 - Gateway Timeout | 网关超时，一般是网关代理服务器请求后端服务时，后端服务没有在特定的时间内完成处理请求，一般为服务器过载所致，没有在指定的时间内返回数据给代理服务器




<br>
<br/>


## 1xx

1xx（临时响应），表示临时响应并需要请求者继续执行操作。

状态码 | 描述
- | -
100 - Continue |  请求者应当继续提出请求
101 - Switching Protocols | 请求者要求服务器更换协议，服务器已确认并准备更换



<br/>
## 2xx

2xx（成功），表示成功处理了请求。

状态码 | 描述
- | -
200 - OK | Server已成功处理了请求
201 - Created | 请求成功并且Server创建了新的资源
202 - Accepted | Server以接受请求，但尚未处理
203 - Non-Authoritative Information | Server已成功处理了请求，但返回的信息可能来自另一个来源
204 - No Content | Server成功处理了请求，但没有返回任何内容
205 - Reset Content | 没有新的内容，但浏览器应该重置它所显示的内容
206 - Partial Content | 服务器成功处理了部分GET请求



<br>
<br/>


## 3xx

3xx（重定向），表示要完成请求需要进一步操作。

状态码 | 描述
- | -
300 - Multiple Choices | 针对请求，Server可执行多种操作
301 - Moved Permanently | 请求的网页已移动到新位置
302 - Found | Server目前从不同位置的网页响应请求
303 - See Other | 请求者对不同位置使用单独的GET请求来检索时
304 - Not Modified | 自从上次请求后，请求的网页内容未修改过
305 - Use Proxy | 请求者只能使用代理访问请求的网页
307 - Temporary Redirect | Server从不同位置的网页响应请求，但请求者继续使用原有位置进行请求



<br>
<br/>


## 4xx

4xx（请求错误），表示请求可能出错，妨碍了Server的处理。

状态码 | 描述
- | -
400 - Bad Request | Server不理解请求的语法
401 - Unauthorized | 请求要求身份认证
403 - Forbidden | Server拒绝请求
404 - Not Found | Server找不到请求的网页
405 - Method Not Allowed | 请求方法不被允许
406 - Not Acceptable | 无法使用请求的恩日工特性响应请求的网页
407 - Proxy Authentication Required | 请求需要代理授权
408 - Request Timeout | Server等候请求时超时
409 - Conflict | Server在完成请求时发生冲突
410 - Gone | 请求的资源以永久删除
411 - Length Required | Server不接受不含有效内容长度Header的请求
412 - Precondition Failed | Server为满足请求者在请求中设置的一个前提条件
413 – Request Entity Too Large | 请求实体太大，Server无法处理
414 - Request URI Too Long | 请求的URI过长，Server无法处理
415 – 不支持的媒体类型 | 请求的格式不受支持
416 – Requested Range Not Satisfiable | 页面无法提供请求的范围
417 – 执行失败 | Server未满足期望请求Header的要求
451 | 基于法律上的的原因，不能像请求者展示网页内容



<br>
<br/>


## 5xx

5xx（服务器错误），表示服务器在尝试处理请求时发生内部错误。这些错误可能是服务器本身的错误，而不是请求出错。

状态码 | 描述
- | -
500 - Internal Server Error | Server遇到错误，无法完成请求
501 - Not Implemented | Server不具备完成请求的功能
502 - Bad Gateway | Server作为网关或代理时，从upstream收到无效响应
503 - Service Unavailable | Server暂时无法使用
504 - Gateway Timeout | Server作为网关或代理时，没有及时从upstream收到请求
505 - HTTP Version Not Supported | Server不支持请求中所用的HTTP版本


















