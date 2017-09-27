# HTTP协议详解

## 什么是HTTP协议

协议是指计算机通信网络中两台计算机之间进行通信所必须共同遵守的规定或规则，超文本传输协议(HTTP)是一种通信协议，它允许将超文本标记语言(HTML)文档从Web服务器传送到客户端的浏览器。

## Web服务器，浏览器，代理服务器

当我们打开浏览器，在地址栏中输入URL，然后我们就看到了网页。 原理是怎样的呢？
实际上我们输入URL后，我们的浏览器给Web服务器发送了一个Request, Web服务器接到Request后进行处理，生成相应的Response，然后发送给浏览器， 浏览器解析Response中的HTML,这样我们就看到了网页。我们的Request 有可能是经过了代理服务器，最后才到达Web服务器的。

代理服务器就是网络信息的中转站，有什么功能呢？
1. 提高访问速度， 大多数的代理服务器都有缓存功能。
2. 突破限制， 也就是FQ了
3. 隐藏身份。

## URL详解

URL(Uniform Resource Locator) 地址用于描述一个网络上的资源，  基本格式如下：
  schema://host[:port#]/path/.../[;url-params][?query-string][#anchor]

scheme               指定低层使用的协议(例如：http, https, ftp)
host                 HTTP服务器的IP地址或者域名
port# ? ? ? ? ? ? ?  HTTP服务器的默认端口是80，这种情况下端口号可以省略。如果使用了别的端口，必须指明，例如 http://www.cnblogs.com:8080/
path ? ? ? ? ? ? ? ? 访问资源的路径
url-params
query-string       发送给http服务器的数据
anchor-             锚

## HTTP协议是无状态的

http协议是无状态的，同一个客户端的这次请求和上次请求是没有对应关系，对http服务器来说，它并不知道这两个请求来自同一个客户端。 为了解决这个问题， Web程序引入了Cookie机制来维护状态。

## HTTP消息的结构

先看Request 消息的结构，   Request 消息分为3部分，第一部分叫请求行， 第二部分叫http header, 第三部分是body. header和body之间有个空行

## Get和Post方法的区别

Http协议定义了很多与服务器交互的方法，最基本的有4种，分别是GET,POST,PUT,DELETE. 一个URL地址用于描述一个网络上的资源，而HTTP中的GET, POST, PUT, DELETE就对应着对这个资源的查，改，增，删4个操作。 我们最常见的就是GET和POST了。GET一般用于获取/查询资源信息，而POST一般用于更新资源信息.

我们看看GET和POST的区别
1. GET提交的数据会放在URL之后，以?分割URL和传输数据，参数之间以&相连，如EditPosts.aspx?name=test1&id=123456.  POST方法是把提交的数据放在HTTP包的Body中.
2. GET提交的数据大小有限制（因为浏览器对URL的长度有限制），而POST方法提交的数据没有限制.
3. GET方式需要使用Request.QueryString来取得变量的值，而POST方式通过Request.Form来获取变量的值。
4. GET方式提交数据，会带来安全问题，比如一个登录页面，通过GET方式提交数据时，用户名和密码将出现在URL上，如果页面可以被缓存或者其他人可以访问这台机器，就可以从历史记录获得该用户的账号和密码。

## 状态码

Response 消息中的第一行叫做状态行，由HTTP协议版本号， 状态码， 状态消息 三部分组成。
状态码用来告诉HTTP客户端，HTTP服务器是否产生了预期的Response.
HTTP/1.1中定义了5类状态码， 状态码由三位数字组成，第一个数字定义了响应的类别
  1XX  提示信息 - 表示请求已被成功接收，继续处理
  2XX  成功 - 表示请求已被成功接收，理解，接受
  3XX  重定向 - 要完成请求必须进行更进一步的处理
  4XX  客户端错误 -  请求有语法错误或请求无法实现
  5XX  服务器端错误 -   服务器未能实现合法的请求

看看一些常见的状态码

200 OK
  最常见的就是成功响应状态码200了， 这表明该请求被成功地完成，所请求的资源发送回客户端。
302 Found
  重定向，新的URL会在response中的Location中返回，浏览器将会使用新的URL发出新的Request。
  例如在IE中输入http://www.google.com. HTTP服务器会返回304， IE取到Response中Location header的新URL, 又重新发送了一个Request。
304 Not Modified
  代表上次的文档已经被缓存了， 还可以继续使用。
400 Bad Request  客户端请求与语法错误，不能被服务器所理解
403 Forbidden    服务器收到请求，但是拒绝提供服务
404 Not Found    请求资源不存在（输错了URL
500 Internal Server Error 服务器发生了不可预期的错误
503 Server Unavailable 服务器当前不能处理客户端的请求，一段时间后可能恢复正常
点击 https://luosoo.github.io/blog20170621/ 即可预览
