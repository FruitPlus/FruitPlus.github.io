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
		color: red;
		font-weight: bold;
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
	<li>当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（内部属性），指向构造函数的原型对象<span class='red'>(看三号线)</span></li>
	<li><img src="http://FruitPlus.github.io/images/oop/oop01.jpg"></li>
</ul>


###原型链

####简单回顾
<ul>
	<li>每个构造函数都有一个原型对象</li>
	<li>原型对象都包含一个指向构造函数的指针<span class='red'>(constructor)</span></li>
	<li>实例都包含一个指向原型对象的指针<span class='red'>[[prototype]]</span></li>
</ul>

####原型链的理解
	
对象的原型决定了实例的类型，默认情况下，所有对象都是<strong>对象(Object)</strong>的实例
{% highlight css %}
function Book(title, publisher){
this.title = title;
this.publisher = publisher;
}
Book.prototype.sayTitle = function(){
alert(this.title);
};
var book1 = new Book("High Performance JavaScript", "Yahoo! Press");
var book2 = new Book("JavaScript: The Good Parts", "Yahoo! Press");
alert(book1 instanceof Book); //true
alert(book1 instanceof Object); //true
book1.sayTitle(); //"High Performance JavaScript"
alert(book1.toString()); //"[object Object]"
{% endhighlight %}
Book 构造器用于创建一个新的 Book 实例。 book1 的原形 （__proto__） 是 Book.prototype， Book.prototype
的原形是 Object。这就创建了一个原形链，book1 和 book2 继承了它们的成员。






