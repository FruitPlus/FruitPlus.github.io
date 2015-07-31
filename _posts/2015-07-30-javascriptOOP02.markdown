---
layout: post
title: 面向对象之原型
category: 
description: 
tags:
- javascript
- 面向对象
- 面试
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

####实现原型链的基本模式
	
对象的原型决定了实例的类型，默认情况下，所有对象都是<strong>对象(Object)</strong>的实例
前面例子中展示的原型链还少一环。我们知道，所有引用类型默认都继承了 Object ，而
这个继承也是通过原型链实现的。大家要记住，所有函数的默认原型都是 Object 的实例，因此默认原
型都会包含一个内部指针， 指向 Object.prototype 。 这也正是所有自定义类型都会继承 toString() 、
valueOf() 等默认方法的根本原因。所以，我们说上面例子展示的原型链中还应该包括另外一个继承层
次。下图为我们展示了该例子中完整的原型链。
{% highlight css %}
function SuperType(){
this.property = true;
}
SuperType.prototype.getSuperValue = function(){
return this.property;
};
function SubType(){
this.subproperty = false;
}
//继承了 SuperType
SubType.prototype = new SuperType();
SubType.prototype.getSubValue = function (){
return this.subproperty;
};
var instance = new SubType();
alert(instance.getSuperValue()); //true
{% endhighlight %}

<img src="http://FruitPlus.github.io/images/oop/oop02.jpg">

####hasOwnProperty()  : 看是不是对象自身下面的属性

<strong>hasOwnProperty()只搜索实例，不搜索原型</strong>

<strong>in操作符实例和原型都搜索</strong>
{% highlight css %}
var book = {
title: "High Performance JavaScript",
publisher: "Yahoo! Press"
};
alert(book.hasOwnProperty("title")); //true
alert(book.hasOwnProperty("toString")); //false
alert("title" in book); //true
alert("toString" in book); //true  因为最外层是Object 
{% endhighlight %}


####constructor:查看对象的构造函数
{% highlight css %}
function Aaa(){
}
var a1 = new Aaa();

alert( a1.constructor );  //Aaa

var arr = [];
alert( arr.constructor == Array );  //true*/


//Aaa.prototype.constructor = Array; 可以修改的，不过没有必要


{% endhighlight %}


####For in 的时候有些属性是找不到的
{% highlight css %}
function Aaa(){
}

Aaa.prototype.name = 10;

for( var attr in Aaa.prototype ){
	alert(attr);  //只有name   constructor 是for in找不到的
}
{% endhighlight %}

####instanceof : 对象与构造函数在原型链上是否有关系

{% highlight css %}
function Aaa(){
}

var a1 = new Aaa();
//alert( a1 instanceof Array );  //false
//alert( a1 instanceof Object );  //true  因为最外层是Object


var arr = [];

alert( arr instanceof Array ) //true

{% endhighlight %}


####toString() :  object上的方法
{% highlight css %}
var arr=[1,2,3]		//系统对象
	// alert(arr.toString()) //string
	// alert(arr.toString==Array.prototype.toString) //true //方法是相同的

	function Aaa(){

	}
	var aaa=new Aaa()	//自己写的对象 

	// alert(aaa.toString==Array.prototype.toString) //false 证明了系统对象的toStirng不同于自己写的对象的toStirng
	// alert(aaa.toString==Object.prototype.toString)//true	自己写的对象的toStirng属于最外层的Object

	// alert(aaa.toString())//[object object]
	// alert(arr.toString())// 1,2,3
	// alert(Array.prototype.toString.call(arr))// 1,2,3  并不是 [object Array] ，因为这个toString是Array下的

	// alert(Array.prototype.toString.call(aaa))//[object object]
	// alert(Object.prototype.toString.call(arr))//[object Array]
	// alert(Object.prototype.toString.call(aaa))//[object object]

	// //综上所得，系统对象有个 toStirng()方法 
	// 自己写的对象 有个 toString()方法 
	//Object.prototype.toString.call(o)这个方法可以判别任何变量的类型 均会返回 [object 类型]
	//Array.prototype.toString.call(o) 也可以,但如果是Array类型的，它则会直接 返回字符串

	//经测试, Number.prototype.toString.call(o)

	//		  Function.prototype.toString.call(o)
	//		  Boolean.prototype.toString.call(o)
	//           .
	//           .
	//           .
	//   	  等都不可以可以判别任何变量的类型    但可以返回自己的类型的字符串//因为他们的toString都是自带的，并不是用Object的
	// 如   alert(Number.prototype.toString.call(num)) //255


	//		所以还是用Object.prototype.toString.call(o)	最妥当

{% endhighlight %}







####最好使用toString方法判别数据类型

因为在一些特殊情况下，constructor和instanceof不管用
{% highlight css %}

window.onload = function(){
	
	var oF = document.createElement('iframe');
	document.body.appendChild( oF );
	
	var ifArray = window.frames[0].Array;
	
	var arr = new ifArray();
	
	//alert( arr.constructor == Array );  //false
	
	//alert( arr instanceof Array );  //false
	
	alert( Object.prototype.toString.call(arr) == '[object Array]' );  //true
	
	
};

{% endhighlight %}