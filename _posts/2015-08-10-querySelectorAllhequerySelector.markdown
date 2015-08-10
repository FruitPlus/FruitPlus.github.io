---
layout: post
title: querySelectorAll和querySelector
category: 
description: 
tags:
- javascript
- 性能
---
<p>当我们在操作特定元素的时候，通常都会采用getELementById()和getElementsByTagName()，而当操作元素列表的时候，则可能需要组合他们并遍历返回节点操作。这种获取节点的操作过程效率低下
	现在<STRONG>比较新的浏览器</STRONG>出提供了名为<STRONG>querySelectorAll()</STRONG>和<STRONG>querySelector()</STRONG>的方法
</p>
<p>
	querySelectorAll()方法是使用作为<strong>CSS选择器</strong>作为参数，返回一个NodeList——包含着匹配节点的类数组对象，这个方法不会返回HTML集合，所以返回的节点不会对应实时的文档结构，避免了HTML集合引起的性能问题
</p>


<a class="jsbin-embed" href="http://jsbin.com/ribubekoja/embed?html,output">Document on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?3.34.1"></script>

