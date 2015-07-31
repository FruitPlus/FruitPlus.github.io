---
layout: post
title: 面向对象之原型
category: 
description: 
tags:
- javascript
- 面向对象
---

<style type="text/css">
	.red{
		color: red
	}
</style>

- 这里是一个目录
{:toc}

###原型的理解
<ul>
	<li>无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个 prototype
属性，这个属性指向函数的原型对象。<span class='red'>(看一号线)</span></li>
	<li>在默认情况下，所有原型对象都会自动获得一个 constructor（构造函数）属性，这个属性包含一个指向prototype 属性<strong>所在函数</strong>的指针<span class='red'>(看二号线)</span></li>
	<li>创建了自定义的构造函数之后，其原型对象默认只会取得 constructor 属性；至于其他方法，则
都是从 Object 继承而来的。</li>
	<li>当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（内部
属性） ，指向构造函数的原型对象<span class='red'>(看三号线)</span></li>
</ul>


	<img src="http://FruitPlus.github.io/images/oop/oop01.jpg">
