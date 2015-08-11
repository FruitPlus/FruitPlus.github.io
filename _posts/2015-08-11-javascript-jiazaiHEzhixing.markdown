---
layout: post
title: 加载和执行
category: 
description: 
tags:
- javascript
- 性能
---

{:toc}
###脚本位置
<p>
	当浏览器遇到<strong> < script ></strong>标签的时候，浏览器会停止处理页面，先执行Javascript代码，然后再继续解析和渲染页面浏览器。
	浏览器必须先花时间下载并执行代码。在这个过程中，页面渲染和用户交互完全被阻塞
</p>
<p>
	HTML4规范指出 < script>标签可以放在 HTML 文档中 的 , <strong>< head ></strong> 或<strong>< body ></strong>中，并允许出现多次。
</p>
<p>通常这种情况，许多人都会把会在head加载javascript文件，但这样页面的性能问题会很明显因为
	<strong>浏览器在解析到< body>标签之前，不会渲染页面的任何部分</strong>
</p>





<!-- ###组织脚本
###无阻塞的脚本
####延迟的脚本
####动态脚本元素
####XMLHttpRequest脚本注入
####推荐的无阻塞模式
 -->
