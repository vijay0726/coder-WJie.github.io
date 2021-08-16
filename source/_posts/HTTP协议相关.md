---
title: HTTP协议相关
---

## 一 . HTTP 基本知识
### 1. HTTP是什么？


HTTP是超文本传输协议。
超文本传输协议就是，【超文本】【传输】【协议】。
<font color=dpurple>超文本：</font>即指【超越了文本的文本】，是图片，文本，视频，尤其关键的**超链接**等。HTML也是超文本，虽然它本身是文字文件，但它包含了图片，超链接，视频等超文本内容。
<font color=dpurple>传输：</font>传输很好理解，就像我们将一个东西从A点搬到B点，或从B点搬到A点。
<font color=blue>可别小看这个简单的动作，它至少包含了两项重要的信息。</font>
<font color=blue>其一，传输是双向的。</font>
可以从A点到B点，同样也可以从B点到A点。
即，浏览器可以向服务器传输信息，服务器也可以向浏览器传输信息。
<font color=blue>其二，传输可以有中转点。</font>
信息可以从A点 经C点 最终传输到 B点。
就像第一排的同学想将纸条传递给最后一排的同学，中途会经过多次中转（中间人传递），这样【A <-> B】就成了 【A <-> M <-> N <-> V...<-> B】
<font color=dpurple>协议：</font>协议也不难理解，就像大学毕业会签的三方协议，租房子时签的租房协议。总而言之，协议就是一种 **规范和约定**。
<font color=red>注意：</font>

> 那「HTTP 是用于从互联网服务器传输超文本到本地浏览器的协议 ，这种说法正确吗？

这种说法是错误的，因为也可以是【服务器 < - > 服务器】
<font color=blue>最后</font>我们应该这样描述HTTP是什么：
<font color=dpurple>HTTP是计算机世界里专门在两点之间传输数据的约定和规范。</font>
### 2. HTTP常见状态码
常见状态码分为1/2/3/4/5开头...见下表：
|#|解释说明  |常见状态码 |
|--|--|--|
| 1xx | 提示信息，表示处于处理的中间状态，还需要后续操作 | |
| 2xx |  表示成功，报文已经收到，并被成功处理   |200 201 202    |
| 3xx |    重定向。资源发生改动，需要重新发送请求。  |   301 302 304 |
| 4xx |    客户端错误。请求报文有误，服务端无法处理。  |   400 403 404  |
| 5xx |     服务端内部错误。服务器在处理请求时，内部发生了错误。 | 500 501 502 503   |


### 3. HTTP报文长什么样
那么HTTP传输的报文到底长什么样？
看看菜鸟教程关于http报文
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210414194631989.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjU2NzQyOA==,size_16,color_FFFFFF,t_70#pic_center)
实际开发中的报文demo

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210414194744830.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjU2NzQyOA==,size_16,color_FFFFFF,t_70#pic_center)
客户端发送一个HTTP请求到服务器的请求消息包括以下格式：请求行（request line）、请求头部（header）、空行和请求数据四个部分组成。

请求行：内容包含 请求方法（get/post/...）请求	URL ，http协议及版本
请求头：内容包含 多个请求字段（Accept , Referer , Content-Type , Connection... ）
请求数据（body）：请求数据的主体。

### 4. HTTP常见字段有哪些？
<font color=blue>Host字段：</font>客户端发送请求时，用来指定服务器的域名。

> 例如：Host: www.A.com
> 例如：Host: localhost:8080

<font color=blue>Content-Length字段: </font>服务器在返回数据时，请求头中会包含Content-Length字段，用来说明返回数据的长度，单位为字节。
> 例如：Content-Length: 1000
> 说明这次返回的数据长度为1000字节。

<font color=blue>Connection字段：</font>Connection字段最常用于客户端发送请求时要求服务器使用TCP持久链接，以便其他请求复用。

<font color=blue>HTTP1.1中默认持久连接，不过为了兼容老版本 要在请求头中加上`Connection: Keep-Alive`</font>
<font color=blue>Content-Type字段：</font>服务器响应请求时，使用Content-Type字段告诉客户端这次返回的数据是什么格式。
>例如：Content-Type: text/html; charset=utf-8
>返回的数据是 html格式，且编码格式时UTF-8

<font color=blue>Accept字段：</font>客户端请求的时候，可以使用 Accept 字段声明自己可以接受哪些数据格式。
>Accept: \*/*
>接受所有类型的数据格式

<font color=blue>Content-Encoding字段：</font>字段说明数据的压缩方法，表明服务器返回的数据使用了什么压缩格式。
![图片来自于小林coding](https://img-blog.csdnimg.cn/20210414211320594.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjU2NzQyOA==,size_16,color_FFFFFF,t_70#pic_center)
图片来源：小林coding图解网络（CSDN）

## 二 .  get 与 post 请求

> 说说get 与 post 的区别

<font color=blue>Get：</font>get 方法的含义是从**服务器获取资源**，这个资源可以是，图片，文本，页面等。
比如，我们打开一篇博客，网站就会发送请求获取这篇文章的相关内容数据，文字，图片等。
get只有可读属性，只能从服务器获取数据，不会更改数据，十分安全。
<font color=blue>而 Post方法则恰恰相反，它向URI指定的资源提交数据，数据就放在报文的body里</font>
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021041421282541.gif#pic_center)
例如：你在一篇博客下评论，点击提交，这时客户端就会发起post请求，会将你的评论保存在请求body中，拼接好请求头，通过TCP协议发送给服务器。
>Get 和 Post 方法都是安全和幂等的吗？

在这里说明一下安全和幂等的概念：

 - 在HTTP协议里，【安全】是指请求方法不会破坏服务器上的资源。
 - 所谓【幂等】，是指多次执行相同的操作，结果是相同的。

<font color=blue>那么，很明显 Get方法 是【安全且幂等】的，因为它具有只读属性，只能读取服务器上的数据，且无论操作多少次，结果不会更改。
然而， Post方法 是【不安全且不幂等】的，post方法 会【新增或增加数据】，且多次提交数据就会多次创建资源。
</font>
不过，Get方法由于是明文传输，将请求的数据放在URL之后的，肉眼可见，而Post方法是将数据放在HTTP正文中的，可以加密。从这点来说，<font color=blue>Post比Get更安全</font>。Get<font color=blue>提交的数据比较少</font>，最多1024B，而<font color=blue>Post提交的数据更多</font>（理论上无限制，但实际也会受到浏览器，操作系统，服务器处理能力的限制）。


## 三 . HTTP 特性
### 1. HTTP的优点有哪些?
<font color=blue> HTTP最突出的优点是：【简单，灵活，易于扩展，可跨平台】</font>
<font color=dpurple>简单：</font>
HTTP基本的报文格式就是`Header + Body`，头部信息也是键值对`key：value`的形式。简单文本的形式，易于理解。
<font color=dpurple>灵活和易于扩展：</font>
HTTP协议里的请求方法，状态码，头部字段等每个组成要求都没有被固定死，都**支持自定义和扩充**。
同时，由于HTTP工作在应用层（OSI第七层），则它的**下层可随意变化**。
HTTPS就是在HTTP与TCP
<font color=dpurple>可跨平台：</font>
从浏览器，Web网站到手机app（购物，社交），游戏（吃鸡，王者）HTTP的应用遍地开花。同时具有天然的跨平台优势。
### 2. 那么，它有哪些缺点呢？
<font color=red>首先，HTTP协议里有一把优缺点一体的双刃剑【无状态，明文传输】</font>
<font color=blue> 同时，还有一大缺点【不安全】</font>

<font color=dpurple>无状态双刃剑：</font>

无状态的**好处**，由于HTTP具有无状态的特点，所以不用耗费更多的资源来记录状态，这无疑大大节省了服务器的资源，从而将更多的CPU和内存用在对外服务上。

无状态的**坏处**，也是由于HTTP不记录请求，所以在一些关联性比较强的操作上操作会很复杂。

对于无状态的问题，其中一种解决方案是，使用<font color=blue> Cookie技术</font>。

Cookie技术是通过在请求和响应的报文中，加入cookie信息来控制客户端的状态。

就好像，在客户端第一次请求信息的时候，服务器会下发一个装有客户信息的【小贴纸】，等下次在请求的时候，带上这个【小贴纸】服务器就认得了。

<font color=dpurple>明文传输双刃剑：</font>

明文传输的好处在于，开发人员可以更方便的查看传输的信息，通过F12控制台或Wireshark抓包都可以直接肉眼查看，极大的方便了调试工作。
但正是这样，HTTP的明文传输无异于信息裸奔。如果你的账号，密码之类的信息在其中，那么很可能【你号没了】。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210415143814497.gif#pic_center)

<font color=dpurple>不安全：</font>

HTTP比较严重的缺点就是**不安全**。

<font color=blue> 体现在三个方面：</font>
1.信息明文传输，有被**窃听风险**。例如：账号密码泄露，你号没了
2.不验证通信方身份，有**冒充风险**。例如：你访问假的淘宝网站，你钱没了
3.无法验证报文的完整性，有被**篡改风险**。例如：网站被植入恶意广告，你眼没了

没了，都没了。。。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210415144552674.gif#pic_center)
HTTP 的安全问题，可以用 HTTPS 的方式解决，也就是通过引入 SSL/TLS 层，使得在安全上达到了极致。

### 3. 关于HTTP1.1的性能如何？
HTTP 协议是基于 TCP/IP，并且使用了「请求 - 应答」的通信模式，所以性能的关键就在这两点里。
HTTP1.1相对于老版本的改进主要体现在**两点**：<font color=blue>长连接和管道网络运输。
</font>
<font color=dpurple>长连接：</font>

早期的HTTP1.0有一个很大的问题是，每发送一次请求都要重新建立一次TCP连接，而且是串行连接，多了很多无意义的连接和断开，对服务器造成了很大的负担，增加了通信成本。
为了解决上述问题，HTTP1.1提出了**长连接**的通信方式，也叫持久连接。减少了TCP连接和断开的次数，减轻了服务器的负载。

<font color=dpurple>管道网络运输：</font>
HTTP/1.1 采用了长连接的方式，这使得管道（pipeline）网络传输成为了可能。

即可在同一个 TCP 连接里面，客户端可以发起多个请求，只要第一个请求发出去了，不必等其回来，就可以发第二个请求出去，可以**减少整体的响应时间**。

举个例子，客户端需要请求两个资源，以前的做法是，在同一个TCP连接里，先发送A请求，等到服务器响应，客户端收到响应后，再发送B请求。管道机制则允许浏览器同时发送A请求和B请求。

但是，服务器的响应还是有顺序的，如果一个请求因为某种情况阻塞了，则排在它后面的请求都会阻塞，这被称为<font color=blue>队头阻塞</font>。

> <font color=blue>队头阻塞</font>
> 
>「请求 - 应答」的模式加剧了 HTTP 的性能问题。
因为当顺序发送的请求序列中的一个请求因为某种原因被阻塞时，在后面排队的所有请求也一同被阻塞了，会招致客户端一直请求不到数据，这也就是「队头阻塞」。好比上班的路上塞车。


<font color=red>总之 HTTP/1.1 的性能一般般，后续的 HTTP/2 和 HTTP/3 就是在优化 HTTP 的性能。</font>

## 四 . HTTP 与 HTTPS
### 1. HTTP 与 HTTPS 有哪些区别？

 1. HTTP是明文传输，HTTPS是在HTTP和TCP之间加了一个SSL/TLS层，使得报文能够加密传输。
 2. HTTP经过TCP三次握手后就可以进行传输，而HTTPS除此之外还要进行SSL层的握手过程，才可以进行加密传输。
 3. HTTP端口为80，HTTPS端口为443
 4. HTTPS协议需要向CA（证书认证机构）申请证书，来确保服务器的身份可信。


### 2. HTTPS 解决了 HTTP 的哪些问题？
HTTPS解决了上文提到的HTTP的安全问题【窃听风险，篡改风险，冒充风险】。
- <font color=blue> 窃听风险</font>，比如通信链路上可以获取通信内容，用户号容易没。
- <font color=blue> 篡改风险</font>，比如强制植入垃圾广告，视觉污染，用户眼容易瞎。
- <font color=blue> 冒充风险</font>，比如冒充淘宝网站，用户钱容易没。

那么，它是如何解决的呢？

 - <font color=blue> 信息加密</font>，通过**对称加密 和 非对称加密 的 混合加密**的形式，实现信息加密，交互信息无法被窃取。
 - <font color=blue>校验机制</font>，通过**摘要算法**保证数据的完整性，无法篡改内容。
 - <font color=blue> 身份证书</font>，将服务器公钥放入到**数字证书**中，无法冒充网站。


由此可见，HTTPS的极大的强化了HTTP的安全性。
## 五 . HTTP1.0 HTTP1.1 HTTP2 HTTP3的演变
待总结。。。




