### Http

#### 1. Http报文结构

组成结构：`请求行+ 请求头 + 空行 + body`或者`响应⾏、响应头、空⾏、响应体`

<img src="images/image-20220327001144979.png" alt="image-20220327001144979" style="zoom:50%;" />

举个🌰请求报文和响应报文：

<img src="/Users/Mr.Meow/Library/Application Support/typora-user-images/image-20220327001356674.png" alt="image-20220327001356674" style="zoom:50%;" />

<img src="/Users/Mr.Meow/Library/Application Support/typora-user-images/image-20220327001833711.png" alt="image-20220327001833711" style="zoom:30%;" />

##### 起始行

对于请求报文，请求行类似于`GET /home HTTP/1.1`，分别为**方法 路径 协议版本**

对于相应报文，起始行类似于`HTTP/1.1 200 OK`，分别为**协议版本 状态码 原因**

在起始行中，每个部分之间都用空格隔开，最后接上转行

##### header

请求头由键值对组成，每⾏⼀对，键值之间⽤英⽂冒号`:`进行分隔。例如：

```
Content-Type: application/json
Host: www.abc.com
```

##### 空行

空行用来区分header和body，如果说在header中故意加一个空行，那么空行后面的内容都会被视为body

##### 请求体

请求体中放置 POST、PUT、PATCH 等请求方法所需要携带的数据。

通信大概是这个模式：

<img src="/Users/Mr.Meow/Library/Application Support/typora-user-images/image-20220327010344636.png" alt="image-20220327010344636" style="zoom:50%;" />

#### 2. HTTP的请求方法

|  方法   | 功能                                                         |
| :-----: | ------------------------------------------------------------ |
|   GET   | 通常⽤于请求服务器发送/请求某些资源（不要被字面意思欺骗了，可获取也可发送） |
|  POST   | 发送数据给服务器（可发可获取）                               |
|  HEAD   | 请求资源的头部信息, 并且这些头部与 HTTP GET ⽅法请求时返回的⼀致。<br />该请求⽅法的⼀个使⽤场景是在下载⼀个⼤⽂件前先获取其⼤⼩再决定是否要下载, 以此可以节约带宽资源 |
|   PUT   | ⽤于全量修改⽬标资源 (看接口, 也可以用于添加)                |
| DELETE  | ⽤于删除指定的资源                                           |
| OPTIONS | ⽤于获取⽬的资源所⽀持的通信选项 (跨域请求前, 预检请求, 判断目标是否安全) |
|  TRACE  | 追踪请求-响应的传输路径 用于诊断和判断                       |
| CONNECT | HTTP/1.1协议中预留给能够将连接改为管道⽅式的代理服务器<br />(把服务器作为跳板，让服务器代替用户去访问其它网页, 之后把数据原原本本的返回给用户) |
|  PATCH  | ⽤于对资源进⾏部分修改                                       |

#### 3. GET 和 POST的区别

|                  | GET方法                                                      | POST方法                                                     |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **数据传输⽅式** | 通过URL传输数据 (地址栏拼接参数)                             | 通过请求体传输                                               |
| **数据安全**     | 数据暴露在URL中，可通过浏览历史记录、缓存等很容易查到数据信息 <br />get请求会被浏览器主动缓存下来，留下历史记录，而post不会 | 数据因为在请求主体内，<br />所以有⼀定的安全性保证           |
| **数据类型**     | 只允许 ASCII 字符                                            | ⽆限制                                                       |
| **GET⽆害**      | 刷新、后退等浏览器操作是⽆害的                               | 可能会引起重复提交表单                                       |
| **功能特性**     | 安全且幂等（这⾥的安全是指只读特性，就是使⽤这个⽅法不会引起服务器状态变化。<br />**幂等的概念是指同⼀个请求⽅法执⾏多次和仅执⾏⼀次的效果完全相同）** | ⾮安全(会引起服务器端的变化)、**⾮幂等**                     |
| **TCP的角度**    | GET会把请求报文一次性发出去                                  | 把数据分两个 TCP 数据包，先发 header 部分，<br />如果服务器响应 100(continue)， 然后发 body 部分。<br />(但是火狐浏览器的 POST 请求只发一个 TCP 包) |

#### 4. HTTP状态码

RFC 规定 HTTP 的状态码为三位数，被分为五类: 

```
1xx: 表示目前是协议处理的中间状态，还需要后续操作。 
2xx: 表示成功状态。 
3xx: 重定向状态，资源位置发生变动，需要重新请求。 
4xx: 请求报文有误。 
5xx: 服务器端发生错误
```

**1xx :**

| 状态码 | 原因短语            | 说明                                                         |
| ------ | ------------------- | ------------------------------------------------------------ |
| 101    | Switching Protocols | 在 HTTP 升级为 WebSocket 的时候，如果服务器同意变更，就会发送状态 码 101。 |

**成功（2XX）**

| 状态码 | 原因短语        | 说明                                                         |
| ------ | --------------- | ------------------------------------------------------------ |
| 200    | OK              | 表示从客户端发来的请求在服务器端被正确处理，通常在响应体中放有数据。 |
| 201    | Created         | 请求已经被实现，⽽且有⼀个新的资源已经依据请求的需要⽽建⽴<br />通常是在POST请求，或是某些PUT请求之后创建了内容, 进行的返回的响应 |
| 202    | Accepted        | 请求服务器已接受，但是尚未处理，不保证完成请求<br />适合异步任务或者说需要处理时间比较长的请求，避免HTTP连接一直占用 |
| 204    | No content      | 表示请求成功，但响应报⽂不含实体的主体部分                   |
| 206    | Partial Content | 进⾏的是范围请求, 表示服务器已经成功处理了部分 GET 请求<br />响应头中会包含获取的内容范围 (常用于分段下载) |

**重定向（3XX）**

| 状态码  | 原因短语           | 说明                                                         |
| ------- | ------------------ | ------------------------------------------------------------ |
| 301     | Moved Permanently  | 永久性重定向，表示资源已被分配了新的 URL<br />比如，我们访问 **http**://www.baidu.com 会跳转到 **https**://www.baidu.com |
| 302     | Found              | 临时性重定向，表示资源临时被分配了新的 URL, 支持搜索引擎优化<br />首页, 个人中心, 遇到了需要登录才能操作的内容, 重定向 到 登录页 |
| 303     | See Other          | 对于POST请求，它表示请求已经被处理，客户端可以接着使用GET方法去请求Location里的URI。 |
| **304** | **Not Modified**   | **自从上次请求后，请求的网页内容未修改过。<br />服务器返回此响应时，不会返回网页内容。(协商缓存)** |
| 307     | Temporary Redirect | 对于POST请求，表示请求还没有被处理，客户端应该向Location里的URI重新发起POST请求。<br />不对请求做额外处理, 正常发送请求, 请求location中的url地址 |

301 浏览器默 认会做缓存优化，在第二次访问的时候自动访问重定向的那个地址。 302浏览器并不会做缓存优化。

**客户端错误（4XX）**

| 状态码  | 原因短语                        | 说明                                        |
| ------- | ------------------------------- | ------------------------------------------- |
| **400** | **Bad Request**                 | **请求报⽂存在语法错误(（传参格式不正确）** |
| 401     | UnAuthorized                    | 权限认证未通过(没有权限)                    |
| 403     | Forbidden                       | 表示对请求资源的访问被服务器拒绝            |
| 404     | Not Found                       | 表示在服务器上没有找到请求的资源            |
| 408     | Request Timeout                 | 客户端请求超时                              |
| 409     | Confict                         | 请求的资源可能引起冲突                      |
| 413     | Request Entity Too Large        | 请求体的数据过大                            |
| 414     | Request-URI Too Long            | 请求行里的 URI 太大                         |
| 429     | Too Many Request                | 客户端发送的请求过多                        |
| 431     | Request Header Fields Too Large | 请求头的字段内容太大                        |

**服务端错误（5XX）**

| 状态码 | 原因短语                   | 说明                                                         |
| ------ | -------------------------- | ------------------------------------------------------------ |
| 500    | Internal Sever Error       | 表示服务器端在执⾏请求时发⽣了错误                           |
| 501    | Not Implemented            | 请求超出服务器能⼒范围，例如服务器不⽀持当前请求所需要的某个功能，<br />或者请求是服务器不⽀持的某个⽅法 |
| 503    | Service Unavailable        | 表明服务器暂时处于超负载或正在停机维护，⽆法处理请求         |
| 505    | Http Version Not Supported | 服务器不⽀持，或者拒绝⽀持在请求中使⽤的 HTTP 版本           |

#### 5. HTTP的特点

1. 语义自由：只规定了基本格式，比如空格分割，换行分隔字段等
2. 传输形式多样：不仅仅可以传输文本，还可以传输图片、视频、音频等等
3. 请求-应答的模式：一发一收，有来有回
4. 无状态：通信过程的上下文都是独立且无关的，默认是不保留状态信息的，这个无状态在长连接中会传输大量重读的信息，就会造成资源浪费，但是如果只是为了获取一些数据，而不需要上下文信息的时候就反而可以减小网络开销
5. 明文传输：报文（主要是头部）用的是文本形式，而不是二进制，方便调试，但是也容易暴露信息，一旦被黑可获取就造成信息的泄露。比如WIFI陷阱就是用http明文传输的缺点，诱导连上热点然后抓用户流量获取信息。
6. http是基于TCP链接的，当开启长连接的时候，共用一个TCP链接，一个时间只能处理一个请求，当有请求消耗时间过长，请求就很容易处于阻塞状态，这就是队头阻塞问题

#### 6. URI

URI, 全称为(Uniform Resource Identifier), 也就是统一资源标识符，它的作用很简单，就是区分互联网 上不同的资源。 但是，它并不是我们常说的网址 , 网址指的是 URL , 实际上 URI 包含了 URN 和 URL 两个部分，由于 URL 过于普及，就默认将 URI 视为 URL

<img src="/Users/Mr.Meow/Library/Application Support/typora-user-images/image-20220327010657425.png" alt="image-20220327010657425" style="zoom:70%;" />

scheme 表示协议名，比如 http , https , file 等等。后面必须和 :// 连在一起。 

user:passwd@ 表示登录主机时的用户信息，不过很不安全，不推荐使用，也不常用。

host:port表示主机名和端口。 path表示请求路径，标记资源所在位置。 

query表示查询参数，为 key=val 这种形式，多个键值对之间用 & 隔开。 

fragment表示 URI 所定位的资源内的一个锚点，浏览器可以根据这个锚点跳转到对应的位置。

举个🌰: 这个 URI 中，

```
https://www.baidu.com/s?wd=HTTP&rsv_spt=1
```

 https 即 scheme 部分， www.baidu.com 为 host:port 部分（注意，http 和 https 的默 认端口分别为80、443）,  /s 为 path 部分，而 wd=HTTP&rsv_spt=1 就是 query 部分。

#### 7. Accept 系列字段

对于 Accept 系列字段的介绍分为四个部分: 数据格式、压缩方式、支持语言和字符集。

##### 数据格式

HTTP支持很多数据格式，但是这么多数据格式，客户端是怎么知道是什么格式的呢？

首先先了解一下**MIME(Multipurpose Internet Mail Extensions, 多用途 互联网邮件扩展)。**最先使用在邮件系统中，使邮件可以发任意类型的数据，这对HTTP也是通用的。HTTP从**MIME type**中取了一部分来标记报文body部分的数据类型，发送端用`Content-Type`来表示数据类型，接收端用`Accept`字段来表示想收到的特定类型的数据。

具体而言，这两个字段的取值可以分为下面几类: text：

```
text: text/html, text/plain, text/css 等 
image: image/gif, image/jpeg, image/png 等 
audio/video: audio/mpeg, video/mp4 等 
application: application/json, application/javascript, application/pdf, application/octet-stream
```

##### 压缩方式

以上的那些数据都是会进行编码压缩的，`Content-Encoding`表示了发送端采取什么方式压缩，`Accept-Encoding`表示了接收端接收什么样的压缩方式，这个字段主要有这几种：

> gzip: 当今最流行的压缩格式 
>
> deflate: 另外一种著名的压缩格式 
>
> br: 一种专门为 HTTP 发明的压缩算法

##### 支持语言

在一些国际化的方案中，可以用来制定支持的语言，发送端用`Content-Language`，接收方用`Accept-Language`

```js
// 发送端
Content-Language: zh-CN, zh, en
// 接收端
Accept-Language: zh-CN, zh, en
```

##### 字符集

在接收端，可以用`Accept-Charset`指定可以接收的字符集，而在发送端，这个字段是被放在了`Content-Type`中，以charset属性来制定

```js
// 发送端
Content-Type: text/html; charset=utf-8
// 接收端
Accept-Charset: charset=utf-8
```

##### 总结

<img src="/Users/Mr.Meow/Library/Application Support/typora-user-images/image-20220327014125370.png" alt="image-20220327014125370" style="zoom:50%;" />

#### 8. 定长和不定长的数据，HTTP是如何传输的？

##### 定长包体

对于定长的包体，发送端在传输的时候会带上`Content-Length`来指明长度，如果长度正确，则浏览器中显示相应的内容，如果传输的内容长度大于length，那么超出的部分就会被截断，如果内容长度小于length，那么浏览器可能就无法显示了，传输失败。

##### 不定长包体

头部字段`Transfer-Encoding: chunked`表示分块传输数据

这个字段会自动产生两个效果：1. content-length会被忽略，2. 基于长链接会推送动态内容

#### 9. HTTP1.X的Keep-alive

作用：使得客户端到服务端的连接持续有效（长连接），对服务器后续的请求不需要重新建立连接

早期的HTTP/1.0每一次请求都要建立连接，这个过程会消耗资源和时间，为了减少资源消耗，缩短响应时间，就需要复用连接，也就是在请求头中加入`Connection: keep-alive`告诉服务器，这个请求响应结束后不要关闭连接，后面还要继续交流。

**keep-alive 的优点** (复用连接)

- 较少的 CPU 和内存的占⽤（因为要打开的连接数变少了, 复用了连接） 
- 减少了后续请求的延迟（⽆需再进⾏握⼿） 
- ...

缺点: 因为在处理的暂停期间，本来可以释放的资源仍旧被占用。请求已经都结束了, 但是还一直连接着也不合适

解决：Keep-Alive: timeout=5, max=100

- timeout：过期时间5秒（对应httpd.conf里的参数是：KeepAliveTimeout），

- max是最多一百次请求，强制断掉连接。

  就是在timeout时间内又有新的连接过来，同时max会自动减1，直到为0，强制断掉。

#### 10. HTTP处理大的文件传输

对于几百M甚至更大的文件来说，如果一下都传输，会造成很长的等待时间，影响用户体验，对于这种情况，HTTP可以用范围请求来允许客户端仅仅请求资源的一部分。前提是服务器支持**范围请求**，如果要用这种功能，如何知道呢？服务器这边的响应头里面有这样一个字段

```js
Accept-Ranges: none //告知客户端这里是支持范围请求的
```

**Range字段拆解**

客户端需要请求哪一部分，可以用`Range`这个请求头字段确定，格式为`bytes:x-y`

> 0-499表示从开始到第 499 个字节。 
>
> 500- 表示从第 500 字节到文件终点。 
>
> -100表示文件的最后100个字节。

服务器收到请求后，先验证范围对不对，如果越界就返回416，否则就读取相应的范围并返回206，服务器收需要添加`Content-Range`字段，这个跟请求头有点差异，分单段数据和多段数据

```js
// 单段数据
Range: bytes=0-9
// 多段数据
Range: bytes=0-9, 30-39
```

**单段数据**

对于单段数据的请求，返回的响应如下:

```js
HTTP/1.1 206 Partial Content
Content-Length: 10
Accept-Ranges: bytes
Content-Range: bytes 0-9/100   //0-9 表示请求的返回，100 表示资源的总大小

i am xxxxx
```

**多段数据**

```js
HTTP/1.1 206 Partial Content
Content-Type: multipart/byteranges; boundary=00000010101
Content-Length: 189
Connection: keep-alive
Accept-Ranges: bytes

--00000010101
Content-Type: text/plain
Content-Range: bytes 0-9/96

i am xxxxx
--00000010101
Content-Type: text/plain
Content-Range: bytes 20-29/96

eex jspy e
--00000010101--
```

这里的Content-Type字段后面的两个属性表示：一定是多段请求的数据，响应体中分隔符是00000010101。因此，在多段数据请求的响应体中，一定会用制定分隔符分开，而且在最后的分隔符末尾添加上`--`表示结束。

#### 11. HTTP中如何处理表单数据的提交

在http中，主要有两种提交表单的方式，`Content-Type`的取值有两种application/x-www-form-urlencoded， multipart/form-data。提交表单一般用POST，因此默认讲提交的数据放在请求体中。

**application/x-www-form-urlencoded**

这种方式的表单内容会被编码成以&分割的键值对，字符用URL编码的方式编码

> // 转换过程: {a: 1, b: 2} -> a=1&b=2 -> 如下(最终形式) "a%3D1%26b%3D2"

**multipart/form-data**

这种方式，请求头中`Content-Type`字段里会包含boundary，看10的多段数据请求，而且boundary的值由浏览器默认指定

> Content-Type: multipart/form-data;boundary=---- WebkitFormBoundaryRRJKeWfHPGrS4LKe 。

数据被分成多个部分，每两个部分之间用分隔符分隔，每个部分都有http头部描述子包体，最后分隔符加上`--`

```js
Content-Disposition: form-data;name="data1";
Content-Type: text/plain
data1
----WebkitFormBoundaryRRJKeWfHPGrS4LKe
Content-Disposition: form-data;name="data2";
Content-Type: text/plain
data2
----WebkitFormBoundaryRRJKeWfHPGrS4LKe--
```

##### 小结

**multipart/form-data**最大的特点就是每一个表单元素都是独立的资源描述，在抓包过程中如果没有看到boundary，但是元素确实是被分开了，是因为浏览器和http封装了这个操作。在实际应用中，图片一般都用**multipart/form-data**而不是用**application/x-www-form-urlencoded**，因为没必要做URL编码，会占用更多空间

#### 12. HTTP1.1队头阻塞

##### 什么是HTTP队头阻塞

HTTP传输是请求应答模式，报文必须是一发一收，但是这里面的人物是被放在一个任务队列里串行执行的，一旦队首执行处理太慢，就会阻塞后面的请求处理。

##### 并发连接

一个域名可以分配多个长连接，相当于增加了任务队列的个数，在RFC2616规定客户端最多病发两个，但是现在的浏览器中上限有很多chrome是6个。

##### 域名分片

一个域名可以分多个长连接，那么可以多分几个域名，比如 content1.sanyuan.com 、content2.sanyuan.com等

这样一个 sanyuan.com 域名下可以分出非常多的二级域名，而它们都指向同样的一台服务器，能够并发 的长连接数更多了，事实上也更好地解决了队头阻塞的问题。

#### 13. COOKIE

因为http的请求每次都是独立的，默认是不保留状态的，如果需要保存一些状态，这个时候就引入了cookie。cookie本质上是浏览器里存储的一个小文件，内部是用键值对存储的。向同一个域名下发送请求，都会携带相同的cookie，服务器拿到cookie后进行解析，这样就可以拿到客户端的状态了。而服务端可以通过响应头中的`Set-Cookie` 字段来对客户端写入 Cookie 。举例如下:

```js
// 请求头
Cookie: a=xxx;b=xxx // 响应头 
Set-Cookie: a=xxx 
Set-Cookie: b=xxx
```

##### cookie的属性

cookie的有效期用**Expires**和**Max- Age**来设定，前者表示过期的时间，后者表示用的时间间隔，单位是s，从浏览器收到报文开始计算，如果过期了，那么这个cookie就会被删除，不会发给服务端。

cookie的两个属性**Domain**和**path**, 给 **Cookie** 绑定了域名和路径，在发送请求前发现域名或者路径和这两个属性不匹配，那么就不会带上cookie。其中`/`表示域名下的任意路径都可以用cookie。

如果带上**Security** ，说明只能通过 HTTPS 传输 cookie，如果带上`HttpOnly`就只能通过http传输，不能用js访问，可以预防XSS攻击

相应的，对于 CSRF 攻击的预防，也有 `SameSite` 属性。

`SameSite` 可以设置为三个值， `Strict` 、 `Lax` 和 `None` 。
 **a.** 在 Strict 模式下，浏览器完全禁止第三方请求携带Cookie。比如请求 sanyuan.com 网站只能在sanyuan.com 域名当中请求才能携带 Cookie，在其他网站请求都不能。
 **b.** 在 Lax 模式，就宽松一点了，但是只能在 get 方法提交表单或者 a 标签发送 get 请求的情况下可以携带 Cookie，其他情况均不能。
 **c.** 在 None 模式下，也就是默认模式，请求会自动携带上 Cookie。

##### cookie的缺点

1. 容量小，只有4KB
2. 性能缺陷，紧跟域名，不管这个域名下的某个地址要不要cookie，请求都会带上完整的cookie，如果参数多的话会造成性能浪费，但是可以用domain和path指定的作用域来解决这个问题
3. 安全问题，cookie是文本的形式传输的，被非法获取的话，进行篡改是非常危险的。在HttpOnly为false的情况下cookie是可以用js来获取的

#### 14. Http缓存

web服务缓存大致可以分为：数据库缓存，服务器缓存（代理缓存，CDN服务器缓存），浏览器缓存

浏览器缓存包含：http缓存，indexDB，cookie，localstorage。

在具体了解 HTTP 缓存之前先来明确几个术语：

- 缓存命中率：从缓存中得到数据的请求数  与 所有请求数的比率。理想状态是越高越好。(看所有的请求中, 多少从缓存中读的)

- 过期内容：超过设置的有效时间，被标记为“陈旧”的内容。

- 验证：验证缓存中的过期内容是否仍然有效，验证通过的话刷新过期时间。

- 失效：失效就是把内容从缓存中移除。

**HTTP缓存**:  (优化页面加载的效率, 如果没有缓存策略, 每次重新加载页面, 会非常慢!)

- **强缓存**
- **协商缓存**

