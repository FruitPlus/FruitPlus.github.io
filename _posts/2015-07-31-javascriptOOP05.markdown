---
layout: post
title: 面向对象之拷贝继承
category: 
description: 
tags:
- javascript
- 面向对象
- 面试
---




###继承 : 子类不影响父类，子类可以继承父类的一些功能 ( 代码复用 )

###属性的继承 : 调用父类的构造函数 call

###方法的继承 : for in :  拷贝继承 (jquery也是采用拷贝继承extend)
{% highlight css %}


function CreatePerson(name,sex){   //父类
	this.name = name;
	this.sex = sex;
}
CreatePerson.prototype.showName = function(){
	alert( this.name );
};

var p1 = new CreatePerson('小明','男');
//p1.showName();


function CreateStar(name,sex,job){  //子类
	
	CreatePerson.call(this,name,sex);
	
	this.job = job;
	
}


//CreateStar.prototype = CreatePerson.prototype //对象的引用
     
extend( CreateStar.prototype , CreatePerson.prototype );对象的复制



//CreateStar.prototype.showJob = function(){};  
//如果这里用CreateStar.prototype = CreatePerson.prototype那么父类也会用拥有showJob这个方法

var p2 = new CreateStar('黄晓明','男','演员');

p2.showName();


function extend(obj1,obj2){
	for(var attr in obj2){
		obj1[attr] = obj2[attr];
	}
}

{% endhighlight %}


###对象的复制
CreateStar.prototype = CreatePerson.prototype; 这是引用，会影响到父类
				
				对象   =	对象			这并不是对象的复制，而是引用
				对象   =	值			对象的复制
{% highlight css %}

/*var a = {
	name : '小明'
};

var b = a; 对象   =	对象

b.name = '小强';

alert( a.name );*/ 小强


/*var a = {
	name : '小明'
};

//var b = a;

var b = {};

extend( b , a );	对象   =	值	

b.name = '小强'; 

alert( a.name ); //小明


function extend(obj1,obj2){
	for(var attr in obj2){
		obj1[attr] = obj2[attr];
	}
}*/


var a = [1,2,3];
var b = a;
//b.push(4);

b = [1,2,3,4];

alert(a)

{% endhighlight %}


<a class="jsbin-embed" href="http://jsbin.com/losiwo/embed?html,css,js,output">Document on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?3.34.1"></script>