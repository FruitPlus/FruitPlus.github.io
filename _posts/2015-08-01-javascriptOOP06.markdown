---
layout: post
title: 面向对象之继承的其他形式
category: 
description: 
tags:
- javascript
- 面向对象
- 面试
---




###类式继承-利用构造函数(类)继承的方式

{% highlight css %}

//类 : JS是没有类的概念的 , 把JS中的构造函数看做的类
	
//要做属性和方法继承的时候，要分开继承
	
function Aaa(){   //父类
	this.name = [1,2,3];
}	
Aaa.prototype.showName = function(){
	alert( this.name );
};

function Bbb(){   //子类
	
	Aaa.call(this);
	
}

var F = function(){};
F.prototype = Aaa.prototype; //引用
Bbb.prototype = new F(); //这里相当于 Bbb.prototype=f1=new F() 
// f1为F的实例，[[prototype]]指向了F.prototype  
//Bbb.prototype引用了f1，所以，Bbb.prototype 能够获取 Aaa.prototype，而不影响Aaa.prototype
(因为是实例，而不是引用，仅仅是指向,而不能修改)

Bbb.prototype.constructor = Bbb; //修正指向问题

var b1 = new Bbb();
//b1.showName();
//alert( b1.name );
//alert( b1.constructor );
b1.name.push(4);


var b2 = new Bbb();

alert( b2.name );

{% endhighlight %}



###原型继承-借助原型来实现对象继承对象

{% highlight css %}
var a = {
	name : '小明'
};

var b = cloneObj(a);

b.name = '小强';

//alert( b.name );
alert( a.name );

function cloneObj(obj){
	
	var F = function(){};
	
	F.prototype = obj;
	
	return new F();
	
}

{% endhighlight %}


####拷贝继承:  通用型的  有new或无new的时候都可以

####类式继承:  new构造函数

####原型继承:  无new的对象
