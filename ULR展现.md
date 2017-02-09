从 URL 输入到页面展现发生了什么

最近在看一本关于网络协议的书《[图解HTTP](http://download.csdn.net/download/loneleaf1/9520582)》
当我们在浏览器的地址栏输入 http://www.pwstrick.com ，然后回车，回车这一瞬间到看到页面到底发生了什么呢？
1.  域名解析
2. 建立TCP连接
3. 发起HTTP请求
4. 服务器响应HTTP请求
5. 浏览器渲染页面
自己原先不是很了解，通过读了这本书后了解了些内幕。
接下来将使用工具Chrome、[Fiddler](https://www.telerik.com/download/fiddler)、[Wireshark](https://www.wireshark.org/download.html)。
一、基础概念
**1）TCP/IP是互联网相关的各类协议族的总称**
![](http://upload-images.jianshu.io/upload_images/4640645-820abe9fcc499069.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
**2）TCP/IP分为4层：应用层、传输层、网络层、链路层。**
发送端从应用层网下走，接收端从链路层网上走。
IP（Internet Protocol）：网际协议位于网络层，IP地址可以和MAC地址配对。
ARP（Address Resolution Protocol）：ARP是一种用以解析地址的协议，根据通信方的IP地址反查出对应的MAC地址。
Routing：路由选择，有点像快递公司的送货过程。
TCP（Transmission Control Protocol）：传输控制协议，提供可靠的字节流传输，将大数据分割成报文段（segment），TCP协议能够确认数据最终是否送达到对方。
![](http://upload-images.jianshu.io/upload_images/4640645-0c99047eca3906c0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
**3）数据信息包装**
![](http://upload-images.jianshu.io/upload_images/4640645-a8360f6332e6b333.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
**4）域名解析DNS服务**
DNS（Domain Name System）位于应用层，提供域名和IP地址之间的解析服务。
 
**5）URI和URL**
URI（Uniform Resource Identifier）：统一资源标识符。
URL（Uniform Resoure Locator）：统一资源定位符，通俗的说法是网址。
URI表示**某一互联网资源**，而URL表示**资源地点**，所以URL是URI的子集，下面是几个URI资源。
![](http://upload-images.jianshu.io/upload_images/4640645-c73d9c63b29a4977.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
**6）RFC**
RFC（Request For Comments）：征求修正意见书，RFC是互联网的设计文档。
要是不按照RFC标准执行，就有可能导致无法通信的状况。
 
**7）HTTP**
HTTP是无状态协议，协议对于发送过的请求或响应都不做持久化处理。
HTTP/1.1为了实现保持状态的功能，引入了Cookie。
 
二、域名解析
在《[What really happens when you navigate to a URL](http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/)》中曾提到DNS会先在缓存中查找记录。
浏览器缓存、系统缓存、路由器缓存、ISP DNS 缓存、递归搜索。
![](http://upload-images.jianshu.io/upload_images/4640645-bf4ae15c4625afe1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
三、建立TCP连接
![](http://upload-images.jianshu.io/upload_images/4640645-c3dc8f28f5588739.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/4640645-b28d29b463a622ab.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
**1）发送端发送一个带SYN标志的数据包给对方**
![](http://upload-images.jianshu.io/upload_images/4640645-26947c4aa6029399.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Sequence Number：序号；
Acknowledgment Number：确认号。
 
**2）接收端回传一个带有SYN和ACK标志的数据包以示传达确认信息**
![](http://upload-images.jianshu.io/upload_images/4640645-6f4631a0c4340ba7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
**3）发送端再回传一个带ACK标志的数据包，代表“握手结束”**
![](http://upload-images.jianshu.io/upload_images/4640645-0bd9ff7dc44132eb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
四、发起HTTP请求
HTTP（Hyper Text Transfer Protocol），超文本传输协议，由请求和响应构成。
在书本的第3章介绍了HTTP信息。
**1）请求报文**
![](http://upload-images.jianshu.io/upload_images/4640645-ca0faddd94e65617.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
报文首部内容如下：
![](http://upload-images.jianshu.io/upload_images/4640645-8af3d49bd88f7052.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在书本的第6章中有详细的HTTP首部说明。
“**Connection:keep-alive**”：持久连接，只要任意一端没有明确提出断开，就保持TCP连接状态。
![](http://upload-images.jianshu.io/upload_images/4640645-a47c734f0ee8be69.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
**2）响应报文**
![](http://upload-images.jianshu.io/upload_images/4640645-6663f6d21e62ee21.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
报文首部内容如下：
![](http://upload-images.jianshu.io/upload_images/4640645-0fd1d4670c0ecb7a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
上图中的200是HTTP状态码，在书中的第4章详细介绍了状态码。
![](http://upload-images.jianshu.io/upload_images/4640645-edae512e8234484f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
五、服务器响应HTTP请求
从上面的响应报文中可以看到服务器软件是Nginx，并且请求的是一张PHP页面。
以前曾经写过一篇《[PHP代码的执行](http://www.cnblogs.com/strick/p/5032943.html)》，不过软件用的是[Apache](http://php.net/manual/zh/book.apache.php)。这里就假设是Apache+PHP(fastcgi)架构提供服务。
**1）Apache**
![](http://upload-images.jianshu.io/upload_images/4640645-e980d0840c1aa6ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Apache对HTTP的请求可以分为连接、处理和断开连接3个大的阶段。同时也可以分为上图所示的11个小的阶段。
 
**2）FastCGI**
[FastCGI](https://en.wikipedia.org/wiki/FastCGI)可以让一个客户端，从网页浏览器向执行在Web服务器上的程序请求数据。
比如现在请求的是“index.php”，根据配置文件，Apache知道这个不是静态文 件，需要去找PHP解析器来处理，那么它会把这个请求简单处理后交给PHP解析器。
Apache会传url、查询字符串、POST数据、HTTP header等，而CGI就是规定要传哪些数据、以什么样的格式传递给后方处理这个请求的协议。
 
**3）PHP脚本执行**
PHP程序完成基本的准备工作后启动PHP及Zend引擎， 加载注册的扩展模块。
初始化完成后读取脚本文件，Zend引擎对脚本文件进行词法分析，语法分析。
编译成opcode执行。
 
服务器最终将生成的HTML代码返回给浏览器。
![](http://upload-images.jianshu.io/upload_images/4640645-1837bb1bdcde292d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
六、浏览器渲染页面
从Chrome的网络工具中可以看到，浏览器会先下载HTML代码，再去下载CSS或JS外部资源。
![](http://upload-images.jianshu.io/upload_images/4640645-e18a43e750738a52.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
网上的很多资料显示，浏览器是边下载HTML，边解析HTML的。
有篇文章叫《[How browsers work](http://taligarsiel.com/Projects/howbrowserswork1.htm)》介绍[浏览器内部工作原理](http://kb.cnblogs.com/page/129756/)的，文中提到了浏览器的渲染引擎——[Webkit](https://webkit.org/)。
渲染引擎首先通过网络获得所请求文档的内容，通常以8K分块的方式完成，下面是渲染引擎基本流程：
解析HTML以构建DOM树 -> 构建Render（渲染）树 -> 布局Render树 -> 绘制Render树
![](http://upload-images.jianshu.io/upload_images/4640645-77bc763cbc20f68c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
下图是Webkit的主流程：
![](http://upload-images.jianshu.io/upload_images/4640645-d52e2905d59a32c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
参考资料：
[Wireshark基本用法](http://blog.jobbole.com/70907/)
[当你输入一个网址，实际会发生什么?](http://blog.jobbole.com/33951/)
[一次完整的HTTP事务是怎样一个过程](http://www.linux178.com/web/httprequest.html)
[从输入url到页面加载完的过程中都发生了什么事情](http://blog.aijc.net/server/2015/11/03/%E4%BB%8E%E8%BE%93%E5%85%A5URL%E5%88%B0%E9%A1%B5%E9%9D%A2%E5%8A%A0%E8%BD%BD%E5%AE%8C%E7%9A%84%E8%BF%87%E7%A8%8B%E4%B8%AD%E9%83%BD%E5%8F%91%E7%94%9F%E4%BA%86%E4%BB%80%E4%B9%88%E4%BA%8B%E6%83%85/)
[当在浏览器地址栏输入一个URL后回车，将会发生的事情？](https://www.zhihu.com/question/34873227)
原文转载至：http://www.cnblogs.com/strick/p/5494869.html
