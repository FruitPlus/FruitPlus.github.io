---
layout: post
title: 面向对象之原型
category: 
description: 
tags:
- javascript
- 面向对象
---



- 这里是一个目录
{:toc}

###工厂模式

工厂模式 : 其实就是封装函数
{% highlight css %}


function createPerson(name){

	//alert(this) 指向window 因为是在window下调用的
	
	//1.原料
	var obj = new Object();
	//2.加工
	obj.name = name;
	obj.showName = function(){
		alert( this.name );
	};
	//3.出场
	return obj;
	
}

var p1 = createPerson('小明');
p1.showName();
var p2 = createPerson('小强');
p2.showName();

{% endhighlight css%}


###构造函数
当new去调用一个函数 : 这个时候函数中的this就是创建出来的对象,而且函数的的返回值直接就是this的
<strong>(隐式返回)</strong>

new后面调用的函数 : 叫做<strong>构造函数</strong>
{% highlight css %}

function CreatePerson(name){
	
	this.name = name;
	this.showName = function(){
		alert( this.name );
	};
	
}

var p1 = new CreatePerson('小明');
//p1.showName();
var p2 = new CreatePerson('小强');
//p2.showName();


{% endhighlight css%}