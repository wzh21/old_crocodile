# 2.1 Principles of Network Applications 网络应用原理

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128144747809.png" alt="image-20230128144747809" style="zoom: 33%;" />

> 不同layer的隔离使编写网络应用程序变得简单

## C/S 与 P2P 模式

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128145233471.png" alt="image-20230128145233471" style="zoom: 33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128145327224.png" alt="image-20230128145327224" style="zoom:33%;" />

## 进程通信

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128145538019.png" alt="image-20230128145538019" style="zoom:33%;" />

## 套接字 socket

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128150045090.png" alt="image-20230128150045090" style="zoom:33%;" />

> socket的英文的直译是插座

## 进程寻址

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128150636610.png" alt="image-20230128150636610" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128150653450.png" alt="image-20230128150653450" style="zoom:50%;" />

## 应用层协议定义了:

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128151054981.png" alt="image-20230128151054981" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128151429964.png" alt="image-20230128151429964" style="zoom:50%;" />

##  因特网提供的运输服务

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128151517706.png" alt="image-20230128151517706" style="zoom: 33%;" />

TCP :

+ 发送和接收过程之间的**可靠传输**

+ **流量控制**：发送方不会压倒接收方
+ **拥塞控制**：网络过载时的节流发送器

+ **==面向连接 connection-oriented==**：客户端和服务器进程之间需要设置

+ **不提供**：定时、最小吞吐量保证、安全性

UDP : 

+ 发送和接收过程之间的**不可靠数据传输**

+ **不提供**：可靠性、流量控制、拥塞控制、定时、吞吐量保证、安全性或连接设置。

+ **==无连接的==**

## 课后习题

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128152158710.png" alt="image-20230128152158710" style="zoom:50%;" />

> B,D 是 P2P模式

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128153240143.png" alt="image-20230128153240143" style="zoom:33%;" />

> Real-time delivery : 实时交付
>
> Loss-free data transfer : 无丢失数据传输

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128153440857.png" alt="image-20230128153440857" style="zoom: 33%;" />

> ==**TCP也不能保证实施交付和吞吐量保证**==

# 2.2 The Web and HTTP

## 2.2.1 HTTP概况

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128154224715.png" alt="image-20230128154224715" style="zoom: 33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128154329266.png" alt="image-20230128154329266" style="zoom: 50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128154536671.png" alt="image-20230128154536671" style="zoom: 33%;" />

### 无状态协议(stateless)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128154931897.png" alt="image-20230128154931897" style="zoom:50%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128154925051.png" alt="image-20230128154925051" style="zoom:33%;" />

## 2.2.2 非持续连接和持续连接

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128155225064.png" alt="image-20230128155225064" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128155657008.png" alt="image-20230128155657008" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128155727919.png" alt="image-20230128155727919" style="zoom:33%;" />

> 持续性连接的HTTP可以将2RTT缩减为1RTT
>
> ==**HTTP 1.1比HTTP1.0进步了, 在于可以持续连接(这是"进步", 而不是差异)**==

## 2.2.3 HTTP报文格式

### HTTP请求报文

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128160344452.png" alt="image-20230128160344452" style="zoom:33%;" />

> ==carriage return : 回车==
>
> ==line feed: 换行==

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128160445150.png" alt="image-20230128160445150" style="zoom:33%;" />

> 使用`GET`方法时, 实体行(entity body)为空, 使用`POST`方法时候才有用

### HTTP响应报文

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128163336817.png" alt="image-20230128163336817" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128163358741.png" alt="image-20230128163358741" style="zoom:33%;" />

## 2.2.4 用户与服务器的交互 : cookie

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128164232392.png" alt="image-20230128164232392" style="zoom: 50%;" /> 

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128164247008.png" alt="image-20230128164247008" style="zoom:50%;" />

> **==cookie类似于"*用户*"的身份标识码==**

## 2.2.5 Web缓存 (Web cache)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128165608785.png" alt="image-20230128165608785" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128165619680.png" alt="image-20230128165619680" style="zoom:33%;" />

> **==Web cache又叫做代理服务器==**
>
> Web cache 既是服务器, 又是客户端
>
> server会告诉web cache 一个对象是否可以被cache以及最多cache多长时间 

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128170626357.png" alt="image-20230128170626357" style="zoom:33%;" />

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128170632052.png" alt="image-20230128170632052" style="zoom:33%;" />

### 2.2.6 条件GET方法

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128171108296.png" alt="image-20230128171108296" style="zoom: 33%;" />

**=="条件GET"的意思是GET是有条件的, 当且仅当该对象有更改的时候==**

### HTTP2 和 HTTP3的简要介绍

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128171224290.png" alt="image-20230128171224290" style="zoom:25%;" /><img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128171216916.png" alt="image-20230128171216916" style="zoom:25%;" />

> 缓解HOL阻塞

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128171311838.png" alt="image-20230128171311838" style="zoom:33%;" />

## 课后习题

###  1

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128173128511.png" alt="image-20230128173128511" style="zoom:33%;" />

**An HTTP *server* does not remember anything about what happened during earlier steps in interacting with this HTTP client. **(服务器和客户端反过来说是不对的)

### 2

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128173247730.png" alt="image-20230128173247730" style="zoom:33%;" />

A cookie is a code used by a **server**, carried on a client’s HTTP request, to access information the server had earlier stored about an earlier interaction with this **Web *browser*.** [Think about the distinction between a *browser* and a *person*.]

### 3

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128173443740.png" alt="image-20230128173443740" style="zoom:33%;" />

The HTTP GET request message is used by a web **client** to request a web **server** to send the requested object **from the server to the client.**

### 4

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128173607097.png" alt="image-20230128173607097" style="zoom:33%;" />

**=="条件GET"的意思是GET是有条件的, 当且仅当该对象有更改的时候==**

### 5

#### (1)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128173743103.png" alt="image-20230128173743103" style="zoom: 50%;" />

#### (2)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128173932557.png" alt="image-20230128173932557" style="zoom: 33%;" />

> 关键词 : least prefer

![image-20230128174710961](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128174710961.png)

#### (3)

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128174026249.png" alt="image-20230128174026249" style="zoom: 33%;" />

> ==Yes, because this is a conditional GET, as evidenced by the If-Modified-Since field.==

### 6

![image-20230128175323500](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128175323500.png)

> ==**HTTP 1.1比HTTP1.0进步了, 在于可以持续连接(这是"进步", 而不是差异)**==

### 7

![image-20230128175606806](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128175606806.png)

+ Caching generally provides for a faster page load time at the client, if the web cache is in the client’s institutional network, because the page is loaded from the nearby cache rather than from the distant server.如果web缓存在客户端的机构网络中，缓存通常会在客户端提供更快的页面加载时间，因为页面是从附近的缓存而不是从远程服务器加载的。
+ Caching uses less bandwidth coming into an institutional network where the client is located, if the cache is also located in that institutional network.如果缓存也位于该机构网络中，则缓存使用较少的带宽进入客户端所在的机构网络。

### 8

![image-20230128175854265](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128175854265.png)

+ HTTP/2 allows a large object to be broken down into smaller pieces, and the transmission of those pieces to be interleaved with transmission other smaller objects, thus preventing a large object from forcing many smaller objects to wait their turn for transmission. HTTP/2允许将一个大对象分解为较小的部分，并将这些部分的传输与其他较小对象的传输交错，从而防止大对象迫使许多较小对象等待传输。
+ HTTP/2 allows objects in a persistent connection to be sent in a client-specified priority order. HTTP/2允许以客户端指定的优先级顺序发送持久连接中的对象。

> TLS是HTTP/3新添的功能

### 9

![image-20230128180038525](C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128180038525.png)

> 例如 `404 Not Found`
>
> 包含服务器的名字, 但是不包含服务器的IP地址

### 10

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128180317714.png" alt="image-20230128180317714" style="zoom: 33%;" />

**To indicate to the server that the client has cached this object from a previous GET, and the time it was cached.**

### 11

<img src="C:\Users\weiziheng\AppData\Roaming\Typora\typora-user-images\image-20230128180616498.png" alt="image-20230128180616498" style="zoom:50%;" />

**The cookie value itself doesn't mean anything. It is just a value that was returned by a web server to this client during an earlier interaction.     cookie值本身没有任何意义。它只是web服务器在早期交互期间返回给该客户端的值。**

