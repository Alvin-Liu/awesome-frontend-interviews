> 该篇是web综合部分，主要包含了HTML(HTML5)、http、跨域、页面优化及其他一些概念性的问题

### doctype是什么？标准模式与兼容模式各有什么区别?

<!doctype>声明必须处于HTML文档的头部，在<html>标签之前，HTML5中不区分大小写,它告知浏览器的解析器用什么文档标准解析这个文档。DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现。在兼容模式中，页面以宽松的向后兼容的方式显示,模拟老式浏览器的行为以防止站点无法工作。

### 行内元素、块级元素、空(void)元素分别是什么？常用的有那些？

- 行内元素：行内元素不形成新内容块，即在其左右可以有其他元素，常见的有：a、span、img、b、i、em、strong、label、input、textarea、select、button、sub、sup、q
- 块级元素：块级元素占据其父元素（容器）横向的整个内容区域，常见的有：div、ul、ol、li、dl、dt、dd、h1-h6、p、table、form、hr、iframe、pre，html5增加的有：header、nav、section、article、aside、footer、audio、video
- 空元素：没有内容的 HTML 元素，常见的空元素： br、hr、img 、input 、link 、meta

### href和src区别? title和alt

- href: 指定网络资源的位置（超文本引用），从而在当前元素或者当前文档和由当前属性定义的需要的锚点或资源之间定义一个链接或者关系，在 link和a 等元素上使用
- src: 属性仅仅嵌入当前资源到当前文档元素定义的位置，是页面必不可少的一部分，是引入。在 img、script、iframe 等元素上使用
- title: 既是html标签，又是html属性，title作为属性时，用来为元素提供额外说明信息,通常当鼠标滑动到元素上的时候显示
- alt: alt是html标签的属性，alt属性则是用来指定替换文字，只能用在img、area和input元素中（包括applet元素），用于网页中图片无法正常显示时给用户提供文字说明使其了解图像信息

### iframe有那些缺点？

- iframe会阻塞主页面的Onload事件；
- 搜索引擎的检索程序无法解读这种页面，不利于SEO;
- iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。

使用iframe之前需要考虑这两个缺点。如果需要使用iframe，最好是通过javascript动态给iframe添加src属性值，这样可以绕开以上两个问题。

### html5有哪些新特性、移除了那些元素？如何处理HTML5新标签的浏览器兼容问题？如何区分 HTML 和 HTML5？

HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加。
	  
- 绘画 canvas;
- 用于媒介回放的 video 和 audio 元素;
- 本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失;
- sessionStorage 的数据在浏览器关闭后自动删除;
- 语意化更好的内容元素，比如 article、footer、header、nav、section;
- 表单控件，calendar、date、time、email、url、search;
- 新的技术webworker, websocket, Geolocation;

移除的元素：
	  
- 纯表现的元素：basefont，big，center，font, s，strike，tt，u;
- 对可用性产生负面影响的元素：frame，frameset，noframes；

支持HTML5新标签：

- IE8/IE7/IE6支持通过document.createElement方法产生的标签，可以利用这一特性让这些浏览器支持HTML5新标签，浏览器支持新标签后，还需要添加标签默认的样式。
- 也可以直接使用成熟的框架、比如html5shim

如何区分HTML5： DOCTYPE声明\新增的结构元素\功能元素

### HTML5的离线储存怎么使用，工作原理？

在用户没有与因特网连接时，可以正常访问站点或应用，在用户与因特网连接时，更新用户机器上的缓存文件。

原理：HTML5的离线存储是基于一个新建的.appcache文件的缓存机制(不是存储技术)，通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。

如何使用：

1、 页面头部像下面一样加入一个manifest的属性；

2、 在cache.manifest文件的编写离线存储的资源；

    CACHE MANIFEST
    #v0.11
    CACHE:
    js/app.js
    css/style.css
    NETWORK:
    resourse/logo.png
    FALLBACK:
    / /offline.html

3、 在离线状态时，操作window.applicationCache进行需求实现。

### 浏览器是怎么对HTML5的离线储存资源进行管理和加载的呢？

在线的情况下，浏览器发现html头部有manifest属性，它会请求manifest文件，如果是第一次访问app，那么浏览器就会根据manifest文件的内容下载相应的资源并且进行离线存储。如果已经访问过app并且资源已经离线存储了，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的manifest文件与旧的manifest文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，那么就会重新下载文件中的资源并进行离线存储。离线的情况下，浏览器就直接使用离线存储的资源。

### 如何理解HTML结构的语义化？

HTML结构的语义化是指通过使用包含语义的标签恰当地表示文档结构，语义化的好处：

- 搜索引擎友好
- 便于团队开发和维护
- 去掉样式或样式丢失后，页面也能呈现清晰的结构
- 可以更好的被屏幕阅读器可以读出网内容

### 渐进增强和优雅降级？

- 渐进增强：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。
- 优雅降级 ：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容

### HTTP中GET与POST的区别？

关于这个题目，曾经有一篇文章一度很火：[99%的人都理解错了HTTP中GET与POST的区别](https://mp.weixin.qq.com/s?__biz=MzI3NzIzMzg3Mw==&mid=100000054&idx=1&sn=71f6c214f3833d9ca20b9f7dcd9d33e4#rd)，后来剧情反转，知乎出现一篇：[听说『99% 的人都理解错了 HTTP 中 GET 与 POST 的区别』？？](https://zhuanlan.zhihu.com/p/25028045)

下面引用w3schools的解答：

GET 方法

- 可被缓存
- 保留在浏览器历史记录中
- 可被收藏为书签
- 不应在处理敏感数据时使用
- 有长度限制
- 只应当用于取回数据

POST 方法

- 不会被缓存
- 不会保留在浏览器历史记录中
- 不能被收藏为书签
- 对数据长度没有要求

### 怎么处理跨域？

跨域的处理主要有下面几种方式：

- jsonp跨域
- document.domain + iframe跨域
- location.hash + iframe跨域
- window.name + iframe跨域
- postMessage跨域
- 跨域资源共享（CORS）
- nginx代理跨域
- Nodejs中间件代理跨域
- WebSocket协议跨域

基本使用与分析可以参考：[前端常见跨域解决方案（全）](https://segmentfault.com/a/1190000011145364)

### 如何实现浏览器内多个标签页之间的通信? (阿里)

WebSocket、SharedWorker，也可以调用localstorge、cookies等本地存储方式，监听localstorge的改变来触发一个事件，通过这个事件，控制它的值来进行页面信息通信（注意quirks：Safari 在无痕模式下设置localstorge值时会抛出 QuotaExceededError 的异常）

### webSocket如何兼容低浏览器？(阿里)

- Adobe Flash Socket
- ActiveX HTMLFile (IE)
- 基于 multipart 编码发送 XHR
- 基于长轮询的 XHR

### 在浏览器中输入URL到整个页面显示在用户面前时这个过程中到底发生了什么？

这个题目好像是必考题了，大神可以讲几天，推荐这篇文章：[从输入 URL 到页面加载完成的过程中都发生了什么事情？](http://fex.baidu.com/blog/2014/05/what-happen/ "从输入 URL 到页面加载完成的过程中都发生了什么事情？")

从网上找到的简答版本如下：

1. 浏览器会开启一个线程来处理这个请求，对 URL 分析判断如果是 http 协议就按照 Web 方式来处理;
2. 调用浏览器内核中的对应方法，比如 WebView 中的 loadUrl 方法;
3. 通过DNS解析获取网址的IP地址，设置 UA 等信息发出第二个GET请求;
4. 进行HTTP协议会话，客户端发送报头(请求报头);
5. 进入到web服务器上的 Web Server，如 Apache、Tomcat、Node.JS 等服务器;
6. 进入部署好的后端应用，如 PHP、Java、JavaScript、Python 等，找到对应的请求处理;
7. 处理结束回馈报头，此处如果浏览器访问过，缓存上有对应资源，会与服务器最后修改时间对比，一致则返回304;
8. 浏览器开始下载html文档(响应报头，状态码200)，同时使用缓存;
9. 文档树建立，根据标记请求所需指定MIME类型的文件（比如css、js）,同时设置了cookie;
10. 页面开始渲染DOM，JS根据DOM API操作DOM,执行事件绑定等，页面显示完成

### 为什么利用多个域名来存储网站资源会更有效？

- CDN缓存更方便
- 突破浏览器并发限制
- 节约cookie带宽
- 节约主域名的连接数，优化页面响应速度
- 防止不必要的安全问题(上传js窃取主站cookie之类的)

### 前端优化方式有哪些方式？

前端优化的途径有很多，按粒度大致可以分为两类，第一类是页面级别的优化，第二类则是代码层面的优化。

页面级别的优化：

这个就不得不看看[雅虎军规35条](https://developer.yahoo.com/performance/rules.html)了，中文版的可以看看[毫秒必争，前端网页性能最佳实践](http://www.cnblogs.com/developersupport/p/webpage-performance-best-practices.html)，主要是对雅虎军规35条的实践和总结

> 性能优化的原则是：多使用内存、缓存或者其他方法减少CPU计算、减少网络请求、减少IO

代码层面的优化, 列举部分：

- 少用全局变量
- 用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能
- 缓存DOM节点查找的结果
- 避免使用CSS表达式
- 避免全局查询
- 避免使用with(with会创建自己的作用域，会增加作用域链长度)、eval(容易用错、性能差、容易引起安全问题)、Function(同样性能差，和eval一样，都不利于压缩工具执行压缩)
- 多个变量声明合并
- 避免图片和iframe等的空src。空src会重新加载当前页面，影响速度和效率
- 尽量避免写在HTML标签中写style属性
- 不滥用float,float在渲染时计算量比较大，尽量减少使用

随着CSS3的普遍，同时需要注意优化的点：

- 尽量使用css3动画，开启硬件加速
- 移动端适当使用touch事件代替click事件
- 避免使用css3渐变阴影效果
- 可以用 transform: translateZ(0) 来开启硬件加速
- 不滥用web字体,web字体需要下载，解析，重绘当前页面，尽量减少使用
- 合理使用requestAnimationFrame动画代替setTimeout
- CSS中的属性（CSS3 transitions、CSS3 3D transforms、opacity、canvas、webGL、video）会触发GPU渲染，要合理使用，过渡使用会引发手机过耗电增加