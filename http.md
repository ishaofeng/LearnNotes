#Http知识的学习
***
##一.HTTP协议
1. HTTP使用可靠的数据传输协议,不用担心数据在传输过程中被破坏,复制或产生畸变.
2. Web服务器是Web资源的宿主
3. MIME: HTTP为每种通过Web传输的对象打上名为MIME的数据格式标签.
   MIME是一种文本标记, 表示一种主要的对象类型和一个特定的子类型
    例如: html格式文件 text/html   JPEG格式图片 image/jpeg
###4. URI(Uniform Resource Identifier, URI)统一资源标识符 用来表示服务器资源的名称
分为URL和URN
URL: 统一资源定位符  分为三个部分1) scheme 2) 服务器地址 3) 指定的资源
     http://www.smalllv.cn/js/bootstrap.js
     http://  是scheme
     www.smalllv.cn  是服务器地址
     /js/bootstrap.js  指定的资源
URN: 统一资源名 特定内容的唯一名称使用,与资源所处的位置无关
###1.5. 事务
一个HTTP事务是由一个HTTP请求命令和一个响应结果组成,通讯是通过格式化的HTTP报文完成
####1.5.1 方法: 每条HTTP请求报文都包含一个方法, 告诉服务器执行什么动作 
GET      从服务器向客户端发送命名资源
PUT      将来自客户端的数据存储到一个命名的服务器资源中区
DELETE   从服务器中删除命名资源
POST     将客户端数据发送到一个服务器网管应用程序
HEAD     仅发送命名资源响应中的HTTP首部
> 1. 在不获取资源的情况下了解资源的情况
> 2. 通过查看响应中的状态码, 看看某个对象是否存在
> 3. 通过查看首部,测试资源是否被修改了 

**安全方法:**不产生动作的方法, HTTP请求不会在服务器内部产生什么结果 比如HEAD, GET方法
####1.5.2 状态码: 每一个HTTP响应报文都包含一个状态码, 告知用户请求是否成功.
100~199: 信息提示
200~299: 成功
> * 200 成功
> * 201 已经创建
> * 202 已经接受请求
> * 204 无内容
> * 205 重置当前页面内容

300~399: 重定向
> * 300 多个选择
> * 301 该请求的URL已经被移除使用
> * 302 与301类似, 使用Location首部给出的URL作为临时URL
> * 303 告诉客户端使用另外一个URL来获取资源
> * 304 Not Modified
> * 305 必须通过代理访问资源, 代理位置必须使用Location指定
> * 307 与301类似, 临时重定向

400~499: 客户端错误
> * 400 错误请求
> * 401 未授权
> * 403 禁止访问,不说明禁止的原因
> * 404 没法找到请求的URL
> * 405 请求的方法不允许, 响应的Allow首部说明允许的方法
> * 406 不被接受
> * 407 代理需要认证
> * 408 请求超时
> * 409 请求在资源引发冲突
> * 411 服务器要求报文包含Content-Length
> * 413 请求的实体太大

500~599: 服务端错误
> * 500 服务器遇到一个妨碍它为请求提供服务的错误时
> * 501 
> * 502 Bad Gateway
> * 503 服务当前不可用
> * 504 网关超时
> * 505 HTTP当前版本不支持
###1.6 报文
报文是在HTTP之间发送的数据块
组成: 1) 起始行 对报文进行描述
     2) 首部块 包含属性
         * 通用首部: 既可以出现在请求报文又可以出现在响应报文
         * 请求首部: 提供更多关于请求的信息
         * 响应首部: 提供更多关于响应的信息
         * 实体首部: 描述主体的长度和响应, 或者资源自身
         * 扩展首部: 规范中没有定义的新首部

         **首部延续行:**将长的首部分为多行可以提高可读性, 多出来的每行前面至少要有一个空格或者制表符.
     3) 主体 可选 数据
**请求报文:**
```
    <method> <request-URL> <version>
    <headers>

    <entity-body>
```
**响应报文:**
```
    </version> <status> <reason-phrase>
    <headers>

    <entity-body>
```
一个HTTP首部总是应该以一个空行结束

       
###1.7 连接
   HTTP是应用层协议, 网络通讯细节使用TCP实现
   TCP提供了:
   * 无插座的数据传输
   * 按序传输
   * 未分段的数据流
###1.8 Web的结构组件
   * 代理: 位于客户端和服务器之间的HTTP中间实体
   * 缓存: HTTP的仓库, 使常用页面的副本可以保存在离客户端更近的地方
   * 网关: 连接其它应用程序的特殊Web服务器
   * 隧道: 对HTTP通信报文进行盲转发的特殊代理
   * Agent代理: 发起自动HTTP请求的半智能Web服务器

##二. HTTP连接
###3.1概述
   HTTP连接是通过TCP/IP建立的， TCP为HTTP提供了一条可靠的比特传输通道，从TCP连接一端填入的字节会从
另一端**以原有的顺序**，**正确的**传输出来。
> 向http://www.smalllv.cn/index.html发送GET请求，整个连接过程如下
> 1. 浏览器解析出主机名  www.smalllv.cn
> 2. 浏览器通过DNS查询当前主机名对应的ip地址
> 3. 浏览器获取端口号
> 4. 浏览器发起到解析出来ip地址和端口的tcp连接 
> 5. 浏览器向服务器发送一条HTTP GET报文
> 6. 浏览器从服务器读取HTTP响应报文
> 7. 浏览器关闭连接

    HTTP是协议栈是 HTTP<--TCP<--IP  HTTPS是在HTTP与TCP之间添加了一个TSL或SSL安全层
###3.2TCP连接
   一个HTTP请求过程中建立TCP连接时间以及请求和响应时间占大多数，实际的请求处理时间很短.
*TCP连接的握手时延:*
   TCP连接的建立过程:
> 1. 请求新的TCP连接时，客户端向服务端发送一个小的TCP分组， 这个分组设置了特殊的SYN标记, 说明这是一个连接请求
> 2. 如果服务器接受了连接， 就会对一些连接参数进行计算，并向客户端回送一个TCP分组，这个分组中的SYN和ACK标记都被置位，
    说明请求已经被接受
> 3. 最后客户端向服务器回送一条确认信息， 通知它连接已经建立成功 （确认分组中允许发送数据)

  用户是同TCP接口建立连接过程，中实际执行以上过程，我们感受到的是时延，小的HTTP事务可能会在TCP建立上花费50%的时间.

*延迟确认:*
**每一个TCP段都有一个序列号和数据完整性校验和**, 每个数据段的接收者收到完好的段时，都会向发送者回送确认分组.
如果发送者没有在指定的窗口时间内收到确认信息，发送者就认为分组已经被破坏或者损坏，并重发数据. TCP延时确认算法会在一个
特定的窗口时间内将输出确认再放在缓冲区中，以寻求能够捎带它的输出数据分组，如果那个时间之内没有输出数据分组，确认信息将
直接进行发送,HTTP请求特点(双峰特征请求), 降低了捎带信息的可能， 往往增大时延.
*TCP慢启动:*
TCP连接会随着时间进行自我调谐，起初会限制连接的最大速度，如果数据成功传输，会随时间的推移提高传输的速度，这种调谐称为**TCP慢启动**,
用于防止因特网的突然过载和拥塞. (每成功接受一个分组，发送端就能发送更多分组), 新连接的速度会比已经交换过一定量数据的已调谐连接慢一些.
*Nagle算法与TCP_NODELAY:*
每个TCP段中至少装在了40个自己的标记和首部, 如果TCP发送大量包含少量数据的分组，网络的性能就会严重下降.Nagle算法试图在发送一个分组之前，
将大量TCP数据绑定在一起， 以提高网络效率。Nagle算法鼓励发送全尺寸数据，只有所有数据被确认后才允许发送非全尺寸。如果其它分组还在传输
过程中，就将那部分数据缓存起来，只有当挂起分组被确认，或者缓存中积累足够发送一个全尺寸数据时，才将缓存的数据发送出去。

HTTP中小报文无法填满分组因而不得不产生时延， Nagle算法组织数据发送，等待确认但是确认本身就有时延，因而产生更大的时延。

HTTP应用通常设置TCP_NODELAY禁止Nagle算法.

*TIME_WAIT累积与端口耗尽:*
**TCP连接断开过程:**
> 1. 发起断开一方向另一方发送FIN消息
> 2. 接收方发送确认ACK给发起者
> 3. 一段时间后接收者发送FIN给发起者
> 4. 发送者给接收者发送ACK,接收者关闭连接释放资源， 发送者进入TIME_WAIT状态，并在2ML周期内保留该状态， 之后关闭连接.

TIME_WAIT周期是为了响应由于第四步ack失败，接收端重新发送的FIN， 为连接中离群的段提供网络消失的时间.

###3.3 HTTP连接的处理

* 并行连接: 同时发起多个事务
* 持久连接: 在事务处理结束之后仍然保持在打开状态的TCP连接. (能够避免连接建立时间和慢启动适应时间)
* 并行和持久同时使用

持久连接： 
1) HTTP/1.0+ "keep-alive"连接 
> Connection: Keep-Alive 在HTTP首部设置，打开持久连接, 将一条连接保持在持久状态.
> Keep-Alive首部用来调节keep-alive行为
> > Connection: Keep-Alive
> > Keep-Alive: max-5, timeout=120
> 表示服务器最多还会为另外5个事务保持连接打开状态，或者将打开状态保持到连接空闲了2分钟之后.
服务器支持keep-alive则回送一个`Connection: Keep-Alive`否则不回送.


2) HTTP/1.1 "persistent" 连接
> 连接默认激活， 需要关闭在头部添加 Connection: close
3) 管道化连接


