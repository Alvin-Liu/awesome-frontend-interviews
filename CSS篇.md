### CSS的盒子模型有哪些？

W3C标准盒模型：width = content
IE怪异盒模型：width = content + padding-left + padding-right + border-left + border-right

### 为什么要初始化CSS样式？

因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。

> 最简单的初始化方法就是： * {padding: 0; margin: 0;} （不建议）

### CSS优先级如何计算？

同权重下，优先级就近原则（离被设置元素越近优先级别越高），载入样式以最后载入的为准：

- 内联样式表（标签内部）> 嵌入样式表（当前文件中）> 外部样式表（外部文件中）

不同权重下，优先级关系：

- `!important` > 内联样式 > ID 选择器 > 类选择器 = 属性选择器 = 伪类选择器 > 标签选择器 = 伪元素选择器 > 通配符选择器
- 计算选择符中 ID 选择器的个数（a），计算选择符中类选择器、属性选择器以及伪类选择器的个数之和（b），计算选择符中标签选择器和伪元素选择器的个数之和（c）。按 a、b、c 的顺序依次比较大小，大的则优先级高，相等则比较下一个。若最后两个的选择符中 a、b、c 都相等，则按照“就近原则”来判断

参考：[CSS 样式优先级](https://segmentfault.com/a/1190000003860309)

### 什么是FOUC?如何避免

Flash Of Unstyled Content：用户定义样式表加载之前浏览器使用默认样式显示文档，用户样式加载渲染之后再从新显示文档，造成页面闪烁。解决方法：把样式表放到文档的head

### 考虑下面的代码,`<h1>`和`<p>`元素的`margin-bottom`的值分别应该是多少px？(假设`<h1>`和`<p>`是同级元素)

    html {
      font-size: 100%;
    }
    h1 { 
      font-size: 2em;
      margin-bottom: 1em;
    } 
    p { 
      font-size: 1em;
      margin-bottom: 1em;
    }

`<h1>`的`margin-bottom`是32px，`<p>`的`margin-bottom`是16px

> 这种现象的发生在于`1em`等同于它当前的`font-size`。因为`<h1>`中的`font-size`被设置为了`2em`,其用在`<h1>`内的em来计算的属性，就为`1em = 32px`。(对于大多数的用户(和浏览器)，`font-size`的值为`100%`，就会默认为`16px`，除非用户通过浏览器设置来改变`font-size`的默认值)

**引申部分：**

面试题中，一般em会跟rem、px、vh、vw等单位进行比较使用场景和区别，px、em和rem应该是被比较的比较多的，简单介绍一下：

- px像素。绝对单位，像素px是相对于显示器屏幕分辨率而言的，是一个虚拟单位。是计算机系统的数字化图像长度单位，如果px要换算成物理长度，需要指定精度DPI
- em是相对长度单位，相对于当前对象内文本的字体尺寸。如当前对行内文本的字体尺寸未被人为设置，则相对浏览器的默认字体尺寸。它会继承父级元素的字体大小，因此并不是一个固定的值
- rem是CSS3新增的一个相对单位(root em,根em),使用rem为元素设定字体大小事，仍然是相对大小但相对的只是HTML根元素

推荐阅读：

- 关于vh、vw可以看看大漠的[再聊移动端页面的适配](https://www.w3cplus.com/css/vw-for-layout.html)
- [95%的中国网站需要重写CSS](http://jorux.com/archives/95-websites-of-china-need-to-rewrite-css/)
- [媒体查询--PX,EM or REM?](https://www.w3cplus.com/css/media-query-units.html?utm_source=tuicool&utm_medium=referral)（关于文中提到的[REM vs EM](https://www.w3cplus.com/css/rem-vs-em.html)一文也推荐看一下）

题外：

上次在掘金上看到有人在争关于em的话题，一方说em是相对于父元素的大小，这也是网上被引用的比较多的解释，另一方说em是相对于自己本身的字体大小。争议的根源是`font-size`具有继承性，它的对错请自己分辨，不过有争议是好事，争议让我们更深入的了解问题。（既然说到继承了，面试官会不会说：假设我们认为em是继承的，请用ES6面向对象简单还原一下我们的问题，然后又是其他一堆东西...）

### css清除浮动的几种方式

- `clear:both`
- `overflow`（hidden和auto可以清除浮动，visible不行）
- clearfix

clearfix方法一：

利用:after和:before来在元素内部插入两个元素块,其实现原理类似于clear:both方法(只使用clearfix:after时在跨浏览器兼容问题会存在一个垂直边距叠加的bug)

    .clearfix:before,
    .clearfix:after {
       content: ".";
       display: block;
       height: 0;
       visibility: hidden;
    }
    .clearfix:after {clear: both;}
    .clearfix {zoom: 1;} /* IE < 8 */

clearfix方法二：

Nicolsa在《[Better float containment in IE using CSS expressions](http://nicolasgallagher.com/better-float-containment-in-ie/)》中介绍的方法

    .clearfix:before,
    .clearfix:after {
      content:"";
      display:table;
    }
    .clearfix:after {
      clear:both;
      overflow:hidden;
    }
    .clearfix {
      zoom:1; /* IE < 8 */
    }

参考：[Clear Float](https://www.w3cplus.com/css/clear-float)

### BFC是什么?怎么触发？有什么用？

BFC 即 Block Formatting Contexts (块级格式化上下文)，是 W3C CSS2.1 规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

只要元素满足下面任一条件即可触发 BFC 特性：

- body 根元素
- 浮动元素：float 除 none 以外的值
- 绝对定位元素：position (absolute、fixed)
- display 为 inline-block、table-cells、flex
- overflow 除了 visible 以外的值 (hidden、auto、scroll)

BFC作用：

- 不和浮动元素重叠
- 清除元素内部浮动
- 解决上下相邻两个元素重叠

推荐阅读：

- [BFC和hasLayout](http://www.cnblogs.com/pigtail/archive/2013/01/23/2871627.html)
- [10 分钟理解 BFC 原理](https://zhuanlan.zhihu.com/p/25321647)

### 重绘和回流（重排）是什么，如何优化？

- Reflow（回流）：当Render Tree中的一部分（或全部）因为元素的尺寸、布局、隐藏等改变而需要重新构建。这就称为回流
- Repaint（重绘）：当Render Tree中的一些元素需要更新属性，而这些属性只是影响元素的外观、风格、而不会影响布局的，就是重绘

**重绘（Repaint）不一定会引起回流（Reflow重排），但回流必将引起重绘（Repaint）**

导致Reflow（回流）的情况：

- 页面首次加载
- 添加或者删除可见的DOM元素；
- 元素位置改变；
- 元素尺寸改变——边距、填充、边框、宽度和高度
- 内容改变——比如文本改变或者图片大小改变而引起的计算值宽度和高度改变；
- 页面渲染初始化；
- 浏览器窗口尺寸改变——resize事件发生时；

减少回流、重绘其实就是需要减少对render tree的操作（合并多次多DOM和样式的修改），并减少对一些style信息的请求，尽量利用好浏览器的优化策略。具体方法有：

- 直接改变className，如果动态改变样式，则使用cssText（考虑没有优化的浏览器）
- 让要操作的元素进行”离线处理”，处理完后一起更新
- 不要经常访问会引起浏览器flush队列的属性，如果你确实要访问，利用缓存
- 让元素脱离动画流，减少回流的Render Tree的规模

参考：[页面重绘和回流以及优化](http://www.css88.com/archives/4996)

### rgba()和opacity的透明效果有什么不同？

rgba()和opacity都能实现透明效果，但最大的不同是opacity作用于元素，以及元素内的所有内容的透明度，而rgba()只作用于元素的颜色或其背景色。（设置rgba透明的元素的子元素不会继承透明效果！）

### 如果需要手动写动画，你认为最小时间间隔是多久，为什么？（阿里）

多数显示器默认频率是60Hz，即1秒刷新60次，所以理论上最小间隔为1/60＊1000ms ＝ 16.7ms

### 怎么解决移动端Retina屏幕1px边框问题？

- viewport + rem 实现
- 0.5px方案
- border-image
- background-image
- 多背景渐变实现
- 使用box-shadow模拟边框
- 伪类 + transform 实现

推荐阅读：

- [7种方法解决移动端Retina屏幕1px边框问题](https://www.jianshu.com/p/7e63f5a32636)
- [再谈Retina下1px的解决方案](https://www.w3cplus.com/css/fix-1px-for-retina.html)

### css hack原理及常用hack

由于不同的浏览器和浏览器各版本对CSS的支持及解析结果不一样，以及CSS优先级对浏览器展现效果的影响，我们可以据此针对不同的浏览器情景来应用不同的CSS。

CSS Hack大致有3种表现形式，CSS属性前缀法、选择器前缀法以及IE条件注释法（即HTML头部引用if IE）Hack，实际项目中CSS Hack大部分是针对IE浏览器不同版本之间的表现差异而引入的。

- 属性前缀法(即类内部Hack)：例如 IE6能识别下划线"_"和星号" * "，IE7能识别星号" * "，但不能识别下划线"_"，IE6~IE10都认识"\9"，但firefox前述三个都不能认识。
- 选择器前缀法(即选择器Hack)：例如 IE6能识别*html .class{}，IE7能识别*+html .class{}或者*:first-child+html .class{}。
- IE条件注释法(即HTML条件注释Hack)：针对所有IE(注：IE10+已经不再支持条件注释)： IE浏览器显示的内容 ，针对IE6及以下版本： 。这类Hack不仅对CSS生效，对写在判断语句里面的所有代码都会生效。

[史上最全的CSS hack方式一览](http://zt.pptv.com/normas/macrocms/rgba_IE6_7_8/hack.html)

### css布局

一般布局相关的整理:

- 如何水平居中一个元素（区分单行、多行）
- 如何竖直居中一个元素（区分居中元素有高度和没有高度的情况）
- 左侧固定，右侧自适应
- 右侧固定，左侧自适应
- 两边固定，中间自适应
- Flex布局
- Grid布局

布局相关的文章：

- [盘点8种CSS实现垂直居中水平居中的绝对定位居中技术](http://blog.csdn.net/freshlover/article/details/11579669)
- [【整理】CSS布局方案](https://segmentfault.com/a/1190000010989110)
- [一个完整的Flexbox指南](https://www.w3cplus.com/css3/a-guide-to-flexbox.html)
- [CSS Grid布局这样玩](https://www.w3cplus.com/css3/playing-with-css-grid-layout.html)（深入了解的话，里面链接的文章可以多看看）




