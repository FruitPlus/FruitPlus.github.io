---
layout: post
title: 加载和执行
category: 
description: 
tags:
- javascript
- 性能
---

- 
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
	<strong>浏览器在解析到< body>标签之前，不会渲染页面的任何部分</strong>所以，尽量将脚本放在底部
</p>


<h3>执行JavaScript代码阻塞其他文件的下载</h3>

<img src="http://FruitPlus.github.io/images/xingneng/01.jpg">

<p>
	从图中可以看出一个问题，就是javascirpt文件是不能并行下载的，而且，下载完成后，会有一段延时，这段时间就是下载好的javascript文件的执行过程。
</p>
<p>
	但在IE8、FireFox3.5、Safari4和Chrome2都可以并行下载了。所以 < script>标签在下载外部资源时不会阻塞其他 < script>标签。不过，尽管脚本的下载过程不会相互影响，但页面的解析和渲染仍然要等到javascript代码下载完成并执行才能继续(后面将会用<strong>无阻塞的脚本</strong>解决)
</p>




###组织脚本

<p>由于每个< script>标签初始下载时都会阻塞页面渲染，所以减少页面包含的< script>标签数量有助于改善这一情况。这不仅针对外链脚本，内嵌脚本的数量同样也要限制。浏览器在解析 HTML 页面的过程中每遇到一个< script>标签，都会因执行脚本而导致一定的延时，因此最小化延迟时间将会明显改善页面的总体性能
</p>
<p>
	这个问题在处理外链 JavaScript 文件时略有不同。考虑到 HTTP 请求会带来额外的性能开销，因此下载单个 100Kb 的文件将比下载 5 个 20Kb 的文件更快。也就是说，减少页面中外链脚本的数量将会改善性能。
</p>
<p>
	通常一个大型网站或应用需要依赖数个 JavaScript 文件。您可以把多个文件合并成一个，这样只需要引用一个< script>标签，就可以减少性能消耗。文件合并的工作可通过离线的打包工具或者一些实时的在线服务来实现。
</p>
<p>
	<strong>需要特别提醒的是</strong>，把一段内嵌脚本放在引用外链样式表的<link>之后会导致页面阻塞去等待样式表的下载(意思就是，样式表没有下载完，脚本就不会去加载，从而阻塞页面)。这样做是为了确保内嵌脚本在执行时能获得最精确的样式信息。因此，建议不要把内嵌脚本紧跟在<link>标签后面。
</p>


###无阻塞的脚本

####延迟的脚本

<p>什么是延迟脚本？</p>
<p>延迟脚本是不会阻塞浏览器的其他进程,是并行下载的，并且它会直到DOM加载完成后执行代码(在onload事件之前触发)</p>
<br/>
<p>HTML 4 为< script>标签定义了一个扩展属性：defer。Defer 属性指明本元素所含的脚本不会修改 DOM，因此代码能安全地延迟执行。defer 属性只被 IE 4 和 Firefox 3.5 更高版本的浏览器所支持，所以它不是一个理想的跨浏览器解决方案。在其他浏览器中，defer 属性会被直接忽略，因此< script>标签会以默认的方式处理，也就是说会造成阻塞。然而，如果您的目标浏览器支持的话，这仍然是个有用的解决方案。下面是一个例子</p>

{% highlight css %}
<script type="text/javascript" src="script1.js" defer></script>
{% endhighlight %}



####动态脚本元素

通过文档对象模型(DOM),可以使用javascript动态创建HTML中的所有内容< script>元素与页面其他元素一样，可以非常容易地通过标准 DOM 函数创建：

{% highlight css %}
var script = document.createElement ("script")
script.type = "text/javascript";

//Firefox, Opera, Chrome, Safari 3+
script.onload = function(){
    alert("Script loaded!");
};

script.src = "script1.js";
document.getElementsByTagName("head")[0].appendChild(script);
{% endhighlight %}

<p>
	使用动态创建的脚本节点下载文件时，返回的代码通常会立刻执行，（除了 Firefox 和 Opera，他们将等待此前的所有动态脚本节点执行完毕）。当脚本是“自运行”类型时，这一机制运行正常，但是如果脚本只包含供页面其他脚本调用调用的接口，则会带来问题。这种情况下，您需要跟踪脚本下载完成并是否准备妥善。可以使用动态 < script> 节点发出事件得到相关信息。
</p>
<p>
	Firefox、Opera, Chorme 和 Safari 3+会在< script>节点接收完成之后发出一个 onload 事件。您可以监听这一事件，以得到脚本准备好的通知：
</p>

{% highlight css %}
var script = document.createElement ("script")
script.type = "text/javascript";

//Firefox, Opera, Chrome, Safari 3+
script.onload = function(){
    alert("Script loaded!");
};

script.src = "script1.js";
document.getElementsByTagName("head")[0].appendChild(script);
{% endhighlight %}

<p>
	Internet Explorer 支持另一种实现方式，它发出一个 readystatechange 事件。< script>元素有一个 readyState 属性，它的值随着下载外部文件的过程而改变。readyState 有五种取值：
</p>
<ul>
	<li>“uninitialized”：默认状态</li>
	<li>“loading”：下载开始</li>
	<li>“loaded”：下载完成</li>
	<li>“interactive”：下载完成但尚不可用</li>
	<li>“complete”：所有数据已经准备好</li>
</ul>


{% highlight css %}
var script = document.createElement("script")
script.type = "text/javascript";

//Internet Explorer
script.onreadystatechange = function(){
     if (script.readyState == "loaded" || script.readyState == "complete"){
           script.onreadystatechange = null;
           alert("Script loaded.");
     }
};

script.src = "script1.js";
document.getElementsByTagName("head")[0].appendChild(script);

{% endhighlight %}

<h4>封装一个标准和IE都能实现的方法</h4>
{% highlight css %}
function loadScript(url, callback){
    var script = document.createElement ("script")
    script.type = "text/javascript";
    if (script.readyState){ //IE
        script.onreadystatechange = function(){
            if (script.readyState == "loaded" || script.readyState == "complete"){
                script.onreadystatechange = null;
                callback();
            }
        };
    } else { //Others
        script.onload = function(){
            callback();
        };
    }
    script.src = url;
    document.getElementsByTagName("head")[0].appendChild(script);
}
{% endhighlight %}
<h4>loadScript()函数使用方法</h4>
{% highlight css %}
loadScript("script1.js", function(){
    alert("File is loaded!");
});
{% endhighlight %}

<p>
	您可以在页面中动态加载很多 JavaScript 文件，但要注意，浏览器不保证文件加载的顺序。所有主流浏览器之中，只有 Firefox 和 Opera 保证脚本按照您指定的顺序执行。其他浏览器将按照服务器返回它们的次序下载并运行不同的代码文件。您可以将下载操作串联在一起以保证他们的次序，如下：
</p>

{% highlight css %}
loadScript("script1.js", function(){
    loadScript("script2.js", function(){
        loadScript("script3.js", function(){
            alert("All files are loaded!");
        });
    });
});
{% endhighlight %}

####使用 XMLHttpRequest(XHR)对象

<p>
	此技术首先创建一个 XHR 对象，然后下载 JavaScript 文件，接着用一个动态 < script> 元素将 JavaScript 代码注入页面。
</p>
{% highlight css %}
var xhr = new XMLHttpRequest();
xhr.open("get", "script1.js", true);
xhr.onreadystatechange = function(){
    if (xhr.readyState == 4){
        if (xhr.status >= 200 && xhr.status < 300 || xhr.status == 304){
            var script = document.createElement ("script");
            script.type = "text/javascript";
            script.text = xhr.responseText;
            document.body.appendChild(script);
        }
    }
};
xhr.send(null);

{% endhighlight %}

###总结

减少 JavaScript 对性能的影响有以下几种方法：

<ul>
	<li>将所有的< script>标签放到页面底部，也就是</body>闭合标签之前，这能确保在脚本执行前页面已经完成了渲染。</li>
	<li> 尽可能地合并脚本。页面中的< script>标签越少，加载也就越快，响应也越迅速。无论是外链脚本还是内嵌脚本都是如此。</li>
	<li>
		<ul>采用无阻塞下载 JavaScript 脚本的方法：
			<li>使用< script>标签的 defer 属性（仅适用于 IE 和 Firefox 3.5 以上版本）；</li>
			<li> 使用动态创建的< script>元素来下载并执行代码；</li>
			<li>使用 XHR 对象下载 JavaScript 代码并注入页面中。</li>
		</ul>
	</li>

</ul>






<!-- ####XMLHttpRequest脚本注入
####推荐的无阻塞模式 -->



