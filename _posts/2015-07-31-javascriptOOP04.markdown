---
layout: post
title: 面向对象之基本包装类型
category: 
description: 
tags:
- javascript
- 面向对象
---


基本类型有<strong>String</strong>、<strong>Number</strong>、<strong>Boolean</strong>、<strong>Null</strong>、<strong>undefined</strong>
基本类型是没有属性和方法的

包装对象 : 基本类型都有自己对应的包装对象 :String  Number  Boolean ,但undefined和null没有

基本类型会找到对应的包装对象类型，然后包装对象把所有的属性和方法给了基本类型，然后包装对象消失

{% highlight css %}
var arr=[1,3,5,4]  //这里是arr是Array的实例
var a=arr.sort()
arr.num=5
alert(arr.num) // 5


var str='abc'			
var b=str.charAt(0)   //被String赋予属性和方法，成为一个对象
str.num=6				//对象已经销毁，没有了属性和方法，所以根本没有num这个属性
alert(b)  		//a   	
原本  这里str是个变量，属于基本类型，不是对象，但可以有charAt的方法，因为这里在后台自动完成进行了三个步骤
(1) 创建 String 类型的一个实例；
(2) 在实例上调用指定的方法；
(3) 销毁这个实例。

alert(str.num)  //undefind  


{% endhighlight %}