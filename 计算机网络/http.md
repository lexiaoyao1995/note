* [HTTP](#http)
  * [主要特点](#%E4%B8%BB%E8%A6%81%E7%89%B9%E7%82%B9)
  * [http请求](#http%E8%AF%B7%E6%B1%82)
  * [http响应](#http%E5%93%8D%E5%BA%94)
  * [请求方式](#%E8%AF%B7%E6%B1%82%E6%96%B9%E5%BC%8F)
      * [get](#get)
      * [post](#post)
        * [get和post区别](#get%E5%92%8Cpost%E5%8C%BA%E5%88%AB)
      * [PUT PATCH](#put-patch)
      * [delete](#delete)
  * [header](#header)
  * [状态码](#%E7%8A%B6%E6%80%81%E7%A0%81)
  * [Session和Cookies](#session%E5%92%8Ccookies)
    * [cookie](#cookie)
        * [缺点](#%E7%BC%BA%E7%82%B9)
        * [Http中的相关字段](#http%E4%B8%AD%E7%9A%84%E7%9B%B8%E5%85%B3%E5%AD%97%E6%AE%B5)
        * [cookies字段](#cookies%E5%AD%97%E6%AE%B5)
    * [session](#session)
        * [session实现方式](#session%E5%AE%9E%E7%8E%B0%E6%96%B9%E5%BC%8F)
    * [区别](#%E5%8C%BA%E5%88%AB)
* [http2](#http2)
  * [主要特点](#%E4%B8%BB%E8%A6%81%E7%89%B9%E7%82%B9-1)
* [http3（Quic）](#http3quic)
* [https](#https)
  * [https和http区别](#https%E5%92%8Chttp%E5%8C%BA%E5%88%AB)

# HTTP

超文本传输协议

## 主要特点

1、支持客户/服务器模式

2、简单快速

3、无连接  完成一次请求后关闭连接

4、无状态  对事物没有记忆信息

## http请求

请求行

（请求方法   url   协议版本）   

请求头

（键值对，多种信息）

请求体

## http响应

状态行

（协议版本 状态码  状态码描述）

响应头

（键值对）

响应体

## 请求方式

共9种，GET、POST ，HEAD，OPTIONS、PUT、PATCH、DELETE、TRACE 和 CONNECT

restful常用4种

#### get

GET 方法的首要目的是 **获取资源**

> 方法特点
>
> a) 参数可见
>
> GET 方法的参数是明文可见的包含在 URL 当中，所以说敏感信息不建议使用 GET 方法
>
> 不过也正是因此，所以 GET 方法允许被保存书签
>
> b) 数据类型只允许 ASCII
>
> GET 方法的数据类型只允许是 ASCII 字符，所以说传递 二进制 文件就不可以用 GET 方法了哦
>
> c) 可以保存书签
>
> 当我们访问某一个网站的频率特别高的时候，肯定添加到书签，那其实书签就是依靠 GET 方法来保存的
>
> d) 可以被缓存
>
> GET 方法支持缓存，当本次请求允许被缓存时，会将资源存值本地 cache ，在未过期的情况下直接取本地 cache；缓存过期后视情况而定
>
> e) 参数会保留在浏览器历史记录
>
> 比较直观的感受就是，我们可以在浏览器的历史记录中查看到曾经搜索过的关键字信息
>
> f) 请求长度会受限于所使用的浏览器与服务器
>
> 不同的浏览器对于 GET 请求长度的限制也是不同的，注意这是 浏览器 / 服务器（IE、Chrome、Apache、IIS等） 对于长度的限制，而不是 HTTP 协议

**urlEncode**

> 对于Url来说，之所以要进行编码，是因为Url中有些字符会引起歧义。
>
> Url编码的原则就是使用安全的字符（没有特殊用途或者特殊意义的可打印字符）去表示那些不安全的字符。

#### post

POST 方法的首要目的是 **提交**，POST 方法一般用于添加资源

> 方法特点
> 
> a) 参数不可见，也不会被保存
>
> 所以说 POST 方法是不可以被保存书签的
>
> b) 不能收藏为书签
>
> 理由如上
>
> c) 不可以被缓存
>
> 我要提交的数据被缓存在本地 cache 中想想其实也是没道理的
>
> d) 不会被保存在浏览器历史中
>
> 同样是因为参数不可见
>
> e) 不限制请求长度
>
> 对于 POST 方法这种以 提交 为首要目的的方法，肯定是不可以限制请求长度的
>
> f) 数据类型
>
> 不限，所以说 POST 是可以 提交文件 到服务器的
>
> g) 请求方式
>
> POST 请求与 GET 请求不同，他会首先提交 HEAD 信息，待得到 100 响应后，才会再次将 DATA 提交

**post的四种提交数据的方式**

对应http，header中的Content-Types

| 值                                | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| application/x-www-form-urlencoded | 在发送前编码所有字符（默认）                                 |
| multipart/form-data               | 不对字符编码。在使用包含文件上传控件的表单时，必须使用该值。 |
| application/json                  |                                                              |
| text/plain                        | 空格转换为 “+” 加号，但不对特殊字符编码。                    |

##### get和post区别

get将请求信息放在url，post放在请求体

 get一般做查询，post一般会改变数据库信息

get幂等性，安全性，post没有

get可以被缓存，被存储，post不行

#### PUT PATCH 

**更新资源**

PUT 对后台来说 PUT 方法的参数是一个完整的资源对象，它包含了对象的所有字段

PATCH 对后台来说 PATCH 方法的参数只包含我们需要修改的资源对象的字段

#### delete

删除资源

## header

| 协议头        | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| Accept        | 可接受的响应内容类型（`Content-Types`）。                    |
| Cookie        | 由之前服务器通过`Set-Cookie`（见下文）设置的一个HTTP协议Cookie |
| Authorization | 用于表示HTTP协议中需要认证资源的认证信息                     |

## 状态码

| 1**  | 信息，服务器收到请求，需要请求者继续执行操作   |
| ---- | ---------------------------------------------- |
| 2**  | 成功，操作被成功接收并处理                     |
| 3**  | 重定向，需要进一步的操作以完成请求             |
| 4**  | 客户端错误，请求包含语法错误或无法完成请求     |
| 5**  | 服务器错误，服务器在处理请求的过程中发生了错误 |

| 100  | Continue                        | 继续。[客户端](http://www.dreamdu.com/webbuild/client_vs_server/)应继续其请求 |
| ---- | ------------------------------- | ------------------------------------------------------------ |
| 101  | Switching Protocols             | 切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议 |
|      |                                 |                                                              |
| 200  | OK                              | 请求成功。一般用于GET与POST请求                              |
| 201  | Created                         | 已创建。成功请求并创建了新的资源                             |
| 202  | Accepted                        | 已接受。已经接受请求，但未处理完成                           |
| 203  | Non-Authoritative Information   | 非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本 |
| 204  | No Content                      | 无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档 |
| 205  | Reset Content                   | 重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域 |
| 206  | Partial Content                 | 部分内容。服务器成功处理了部分GET请求                        |
|      |                                 |                                                              |
| 300  | Multiple Choices                | 多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择 |
| 301  | Moved Permanently               | 永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替 |
| 302  | Found                           | 临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI |
| 303  | See Other                       | 查看其它地址。与301类似。使用GET和POST请求查看               |
| 304  | Not Modified                    | 未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源 |
| 305  | Use Proxy                       | 使用代理。所请求的资源必须通过代理访问                       |
| 306  | Unused                          | 已经被废弃的HTTP状态码                                       |
| 307  | Temporary Redirect              | 临时重定向。与302类似。使用GET请求重定向                     |
|      |                                 |                                                              |
| 400  | Bad Request                     | 客户端请求的语法错误，服务器无法理解                         |
| 401  | Unauthorized                    | 请求要求用户的身份认证                                       |
| 402  | Payment Required                | 保留，将来使用                                               |
| 403  | Forbidden                       | 服务器理解请求客户端的请求，但是拒绝执行此请求               |
| 404  | Not Found                       | 服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面 |
| 405  | Method Not Allowed              | 客户端请求中的方法被禁止                                     |
| 406  | Not Acceptable                  | 服务器无法根据客户端请求的内容特性完成请求                   |
| 407  | Proxy Authentication Required   | 请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权 |
| 408  | Request Time-out                | 服务器等待客户端发送的请求时间过长，超时                     |
| 409  | Conflict                        | 服务器完成客户端的 PUT 请求时可能返回此代码，服务器处理请求时发生了冲突 |
| 410  | Gone                            | 客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置 |
| 411  | Length Required                 | 服务器无法处理客户端发送的不带Content-Length的请求信息       |
| 412  | Precondition Failed             | 客户端请求信息的先决条件错误                                 |
| 413  | Request Entity Too Large        | 由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息 |
| 414  | Request-URI Too Large           | 请求的URI过长（URI通常为网址），服务器无法处理               |
| 415  | Unsupported Media Type          | 服务器无法处理请求附带的媒体格式                             |
| 416  | Requested range not satisfiable | 客户端请求的范围无效                                         |
| 417  | Expectation Failed              | 服务器无法满足Expect的请求头信息                             |
|      |                                 |                                                              |
| 500  | Internal Server Error           | 服务器内部错误，无法完成请求                                 |
| 501  | Not Implemented                 | 服务器不支持请求的功能，无法完成请求                         |
| 502  | Bad Gateway                     | 作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应 |
| 503  | Service Unavailable             | 由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中 |
| 504  | Gateway Time-out                | 充当网关或代理的服务器，未及时从远端服务器获取请求           |
| 505  | HTTP Version not supported      | 服务器不支持请求的HTTP协议的版本，无法完成处理               |

## Session和Cookies

**理念**

让Http有状态

### cookie

![](../pic/爱奇艺20190711114948.png)

1、cookie是由服务器发送给服务器的特殊信息，以文本的信息存放在客户端

2、服务器再次请求的时候，会把cookie回发

3、服务器收到后，会解析cookie生成与客户端相对性的内容

##### 缺点

- cookie会被附加在每个HTTP请求中，所以无形中增加了流量。
- 由于在HTTP请求中的cookie是明文传递的，所以安全性成问题。（除非用HTTPS)
- Cookie的大小限制在4KB左右。对于复杂的存储需求来说是不够用的。

##### Http中的相关字段

Set-Cookie

##### cookies字段

> COOKIE主要有哪些字段呢：
>
> 1、name  COOKIE的名字
>
> 2、value  COOKIE对应的值
>
> 3、domain   域名   就是说这个COOKIE对应哪一个域名有效
>
> 4、path     路径 ， COOKIE对应的哪一个路径才会有效
>
> 5、expires/Max-Age 字段为此cookie超时时间。若设置其值为一个时间，那么当到达此时间后，此cookie失效。不设置的话默认值是Session，意思是cookie会和session一起失效。当浏览器关闭(不是浏览器标签页，而是整个浏览器) 后，此cookie失效。


### session

1、服务器端的机制，在服务器上保存信息

2、解析客户端请求并操作sessionid，按需保存状态信息

##### session实现方式

1、使用cookie  jsessionid

2、使用url回写来实现

### 区别

cookie在客户端  session服务器上

session更安全

session会让服务器负担加重

# http2

## 主要特点

多路复用

stream流式交互

流量控制

服务器推送

头部压缩

# http3（Quic）

基于udp协议，但是提供可靠保障和流量控制

避免http2协议的前序包阻塞（hol阻塞）

零rtt建连

fec前向纠错

# https

http被广泛应用于web通信中，但是是以明文形式传输数据，若被拦截截取数据，可直接读取用户信息，因此需要对一些敏感的信息进行加密。

因此http协议基础上加入ssl协议，ssl依靠证书对客户端进行验证，并对数据进行加密。

## https和http区别

1、https需要到ca申请证书

2、http是通过超文本传输协议是明文形式，https通过ssl对数据进行加密

3、默认端口：https 443   http 80