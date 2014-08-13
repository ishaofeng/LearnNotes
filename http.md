#Http知识的学习
***
##一.基础
1. HTTP使用可靠的数据传输协议,不用担心数据在传输过程中被破坏,复制或产生畸变.
2. Web服务器是Web资源的宿主
3. MIME: HTTP为每种通过Web传输的对象打上名为MIME的数据格式标签.
   MIME是一种文本标记, 表示一种主要的对象类型和一个特定的子类型
    例如: html格式文件 text/html   JPEG格式图片 image/jpeg
4. URI(Uniform Resource Identifier, URI)统一资源标识符 用来表示服务器资源的名称
    分为URL和URN
    URL: 统一资源定位符  分为三个部分1) scheme 2) 服务器地址 3) 指定的资源
         http://www.smalllv.cn/js/bootstrap.js
         http://  是scheme
         www.smalllv.cn  是服务器地址
         /js/bootstrap.js  指定的资源
    URN: 统一资源名 特定内容的唯一名称使用,与资源所处的位置无关
5. 事务
   一个HTTP事务是由一个HTTP请求命令和一个响应结果组成,通讯是通过格式化的HTTP报文完成
  1) 方法: 每条HTTP请求报文都包含一个方法, 告诉服务器执行什么动作 
        GET      从服务器向客户端发送命名资源
        PUT      将来自客户端的数据存储到一个命名的服务器资源中区
        DELETE   从服务器中删除命名资源
        POST     将客户端数据发送到一个服务器网管应用程序
        HEAD     仅发送命名资源响应中的HTTP首部
  2) 状态吗: 每一个HTTP响应报文都包含一个状态码, 告知用户请求是否成功.
6. 报文
7. 连接
   HTTP是应用层协议, 网络通讯细节使用TCP实现
   TCP提供了:
   * 无插座的数据传输
   * 按序传输
   * 未分段的数据流
8. Web的结构组件
   * 代理: 位于客户端和服务器之间的HTTP中间实体
   * 缓存: HTTP的仓库, 使常用页面的副本可以保存在离客户端更近的地方
   * 网关: 连接其它应用程序的特殊Web服务器
   * 隧道: 对HTTP通信报文进行盲转发的特殊代理
   * Agent代理: 发起自动HTTP请求的半智能Web服务器
