---
layout: post
category : 读书笔记
title: '高性能网站建设指南'
tagline: ""
tags : [读书笔记, 网站建设, 性能]
---

### 【书名】高性能网站建设指南

### 【内容】

- 规则1减少HTTP请求
- 规则2减少内容发布网络
- 规则3添加Expires头
- 规则4压缩组件
- 规则5将样式表放在顶部
- 规则6将脚本放在底部
- 规则7避免CSS表达式
- 规则8使用外部js和CSS
- 规则9减少DNS查找
- 规则10精简JS
- 规则11避免重定向
- 规则12移除重复脚本
- 规则13配置ETag
- 规则14使AJAX可缓存

并非每个规则都要应用与网站，也不是每个网站都应该用同一种方式运用每一个规则，但每个规则都值得考虑。

<!--break-->

### 【摘录】

一次web请求，就是从浏览器发出一些参数到服务器，然后程序对请求进行处理，再生成浏览器可以识别的内容（HTML，脚本，css...），最后由浏览器将这些内容展现给访问者。

后端用于分析用户请求，执行数据查询并对结果进行组织，形成浏览器可以呈现的内容；前端负责将后端生成的内容通过网络发送给客户端。

HTTP概述

有至少80%的时间花在了显示web页面上，这是在http文档下载完毕后发生的
	
80%的时间花在哪里？如何减少？

查看HTTP流量，开发者工具查看

- 有缓存的场景没有太多的下载活动
- 大量的HTTP请求并行发生，使用多个不同的主机
- 在请求脚本时不会发生并行请求，浏览器下载脚本时会阻塞额外的HTTP请求

黄金法则：

> 只有10%~20%的最终用户响应时间花在了下载HTML文档上。其余80%-90%的时间花在了下载页面上的所有组件上。

HTTP协议是纯文本形式的客户端/服务器协议，由请求和响应构成

请求的类型有GET、POST、HEAD、PUT、DELETE、OPTIONS和TRACE

条件GET请求和304响应有助于页面加载得更快，但仍需要往返请求确定

	If-Modified-Since: Web, 22 Feb.. 
	304 Not Modified
	Expires: Web, 05 Oct...

会和响应的组件一起保存到缓存中，只有组件没过去，就不请求

Keep-Alive

HTTP构建在TCP之上，早期的HTTP请求都要打开socket请求，效率较低持久链接的引入解决了多对一请求服务器导致的socket连接低效问题服务器的响应中使用Connection来指出对Keep-Alive的支持

	Connection: keep-alive

#### 一、减少HTTP请求

减少HTTP请求，图片地图，将多个图片请求合并到一张图片内联图片data:URL模式可以web页面上保护图片但无需额外的HTTP请求合并脚本和样式表

#### 二、减少内容发布网络

如果web服务器离用户更近，则一个HTTP请求的时间将缩短，

web组件服务器离用户更近，则多个HTTP请求的响应时间将缩短

CDN一组分布在不同地理位置的Web服务器

CDN用于发布静态内容

#### 三、为组件添加长久的Expires头

Web页面包含大量的组件，页面访问者会进行很多HTTP请求，但通过一个Expires头，使这些组件可以被缓存，在后续的页面浏览中避免不必要的请求

web服务器使用Expires来告诉客户端可以使用一个组件的当前副本（要求客户端与服务端的时间一致）

HTTP1.1引入Cache-Control头来克服Expires头限制

换一种方式max-age能重写Expires的时间，以相对的方式设置时间

跨浏览器改善缓存的最佳解决方案就是由ExpiresDefault设置的Expires头

使用Expires后更新？

> 最有效的方案是修改其所有链接，这样，全部请求将从原始服务器下载最新的内容

#### 四、压缩组件

web客户端可以通过HTTP请求的Accept-Encoding头来标志对压缩的支持

	Accept-Encoding: gzip,deflate
	响应
	Content-Encoding: gzip 目前最流行做有效的压缩

压缩什么？
	
- HTML文档，脚本和样式表。图片和PDF不应该被压缩，本来就已经压缩，压缩会浪费CPU资源，还可能增加文件大小。通常对大于1K或2KB的文件压缩，节省数据量。

- 压缩成本，服务器用额外的CPU周期完成压缩，客户端要对压缩文件进行解压缩

#### 五、将样式表放在顶部

将样式表放在底部会阻止内容的逐步呈现。为避免当样式变化时重绘页面中的元素，浏览器会阻塞内容的逐步呈现，浏览器会延迟显示任何可视化组件。

对加载页面的时间没有太多的影响，影响更多地是对这些组件顺序的反应。

**为了避免白屏，请将样式表放在顶部**

白屏的出现？

> 样式表在浏览器中的位置不影响页面的加载速度，但影响页面的呈现。
	浏览器尝试修改前端工程师所犯的错误，浏览器延迟呈现，直到样式表下载之后，导致了白屏。

如果样式表仍在加载，构建呈现树就是一种浪费，在所以样式表下载并解析完毕之前无需绘制任何东西。否则在准备好之前显示的内容会遇到FOUC（无样式内容闪烁问题）

当页面逐步加载，文字首先被显示，其次是图片，最后样式表解析之后，已经呈现
的文字和图片要用新的样式重绘了。

#### 六、将脚本放在底部

脚本的出现阻塞了并行下载，所有位于脚本一下的内容呈现都被阻塞了。

#### 七、避免CSS表达式

#### 八、使用外部js和CSS

#### 九、减少DNS查找

影响DNS缓存的因素。查找返回的DNS包含一个存活的时间（Time To Live）值，，该值告诉客户端可以对该记录缓存多久

HTTP协议中的keep-alive可以同时覆盖TTL和浏览器的时间限制。也就是说，只有浏览器和服务端愉快地通信，就没必要进行DNS查找

浏览器对DNS的缓存数量限制，如果用户在短时间访问了大量不同域名的网站，较早的DNS缓存会被浏览器丢弃。

在减少DNS查找和高度并行下载之间权衡。

#### 十、精简JS

精简

> 从代码中移除不要的字符以减少其大小，进而改善加载时间的实践

代码精简之后，所有注释及不必要的空白字符被移除。因为下载的文件变小了，可以改善响应时间

混淆（不建议，如果目的不是抵制反向工程）
	
> 移除注释和空白，同时会改写代码，作为改写的一部分，函数和变量会被更短的字符串替换，代码更加精炼，也更难阅读。

精简CSS

> 精简CSS的节省通常小于精简JS，CSS的注释和空白相对少。最大潜在节省是优化CSS，合并相同类，去除不用的类。
	CSS依赖顺序的本质，层叠样式表的原因决定优化并不简单

#### 十一、避免重定向

重定向（Redirect）用于将用户从一个URL重新路由到另一个URL

301、302是最常用的两种

主要记得重定向会使你的页面变慢

当web服务器向浏览器返回重定向时，响应中就会拥有一个3XX的状态码

html文档头中meta refresh标签
	
	<\meta http-equiv="refresh" content="0; url=http://xxx">

必须重定向，最好的技术是使用的3XXhttp状态码，确保后退按钮能够正确工作

第一个请求是重定向，那么在重定向完毕并且HTML文档下载完毕之前，没有任何东西显示给用户，重定向延迟了整个HTML文档的传输。

缺少结尾的斜线

- URL的结尾必须出现斜线时而没有出现
- 主机名后缺少结尾斜线不会发生重定向，然而在浏览器中最终看到的URL
- 是有结尾斜线的。浏览器进行GET请求必须指定路径，如果没有，就会简单使用文档根路径/

当缺少结尾斜线时发送重定向是很多web服务器的默认行为，
	
注：Apache会，nginx不会

检查web日志发出多少301状态码，是否值得解决缺少结尾斜线问题

将用户从旧的URL转移到新的URl最简单的方式就是重定向，降低了开发复杂性，损害了用户体验

出于美化URL的目的，用容易记得URL重定向不容易记的URL

#### 十二、移除重复脚本

导致不必要HTTP请求和执行JS所浪费的时间

#### 十三、配置或移除ETag

实体标签（Entity Tag）是web服务器和浏览器用于确认缓存组件的有效性的一种机制

服务器在检查缓存的组件是否和原始服务器的组件匹配时有两种方式
	
- 比较最新修改的日期
- 比较实体标签

原始服务器通过last-modified响应头来返回组件的最新修改日期

Etag提供了另外一种方式检测，Etag是唯一标识了一个组件的一个特定版本的字符串（该字符必须用引号引起来）原始服务器使用ETag头响应头来指定组件的ETag为加入验证实体提供了比最新修改日期更为灵活的机制

如果实体依据User-Agent或Accept-Language头而改变，实体的状态可以反应在ETag中。此后，如果浏览器必须验证一个组件，它会私用if-None-Match头将ETag传回原始服务器，如果匹配，就会返回304状态码

问题，对于多台服务器的网站，ETag嵌入的数据会大大地降低有效性验证的成功率。

不同服务器产生的ETag不会匹配，会响应200，导致服务器性能下降

#### 十四、使AJAX可缓存

主动AJAX，被动AJAX

当发起主动AJAX请求时，用户可能仍须等待，要改善性能，优化这些请求很重要。

规则4、9、10、11、13也适用AJAX

缓存Ajax并不像添加一个长久的Expires那么简单，如果产生修改，必须确保修改后不使用缓存，后端应该具有一个表示最后修改的时间戳，并嵌入到Ajax请求的查询字符串中

确保AJax请求遵循性能知道，尤其应具有长久的Expires头

- 在服务端加 header("Cache-Control: no-cache, must-revalidate");(如php中)
- 在ajax发送请求前加上 anyAjaxObj.setRequestHeader("If-Modified-Since","0");
- 在ajax发送请求前加上 anyAjaxObj.setRequestHeader("Cache-Control","no-cache");
- 在 Ajax 的 URL 参数后加上 "?fresh=" + Math.random(); //当然这里参数 fresh 可以任意取了
- 第五种方法和第四种类似，在 URL 参数后加上 "?timestamp=" + new Date().getTime();


















