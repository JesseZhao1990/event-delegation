## 总结事件代理 ##

**事件冒泡**

当一个元素上的事件被触发的时候，比如说鼠标点击了一个按钮，同样的事件将会在那个元素的所有祖先元素中被触发。这一过程被称为事件冒泡；

**事件代理**	
	
事件代理就是用到了事件冒泡的原理和目标元素这两个特性。我们把事件处理器添加到一个元素上，等待它的所有子级元素的事件冒泡上来，然后判断这个事件来自于那个子元素。并进行响应。

**为什么要用事件代理？**

假如一个表格。有几千行。假如我希望点击每个单元格的时候，单元格的背景色进行变化。如果对每个单元格进行绑定，不仅仅性能低下，而且还可能引起内存泄露。如果利用事件代理。仅仅需要对table绑定一个事件就万事大吉了。我只需要在table上监听单击事件。判断此事件的来源。就可以针对性的进行响应了。

再比如，如果你的Dom树中。并不存在某一个元素。但是你又需要绑定事件到该元素上，那又怎么办呢？如果利用事件代理，你只需要把还未在dom书中出现的元素绑到父元素上甚至body上或者document上就ok。


**用原生js实现一个事件代理的Demo**

鼠标点击li。弹出li里的内容	
[演示地址](http://codepen.io/zhaojianxin/pen/ALEqmR)

**html**

```
<ul>
	<li>111111</li>
	<li>222222</li>
	<li>333333</li>
	<li>444444</li>
	<li>555555</li>
	<li>666666</li>
	<li>777777</li>
</ul>

```

**js**

```	
// 获取ul
var ulDom = document.getElementsByTagName("ul");

// 定义绑定事件的方法。主要是处理兼容性
var addEvent = function( obj, type, fn ) { 
	if (obj.addEventListener){   // 能力检测，检测非ie浏览器
		obj.addEventListener( type, fn, false );
	}else if (obj.attachEvent) { // 能力检测，检测ie浏览器
		obj["e"+type+fn] = fn; 
		obj.attachEvent( "on"+type, function() { 
			obj["e"+type+fn](); 
		} ); 
	} 
};

//给ul 绑定click事件
addEvent(ulDom[0],"click",function(e){
	if(e.target.tagName.toLowerCase()=="li"){
		alert(e.target.textContent);
	}
})
```

**用jQuery实现一个事件代理的Demo**

鼠标点击li。弹出li里的内容	
[演示地址](http://codepen.io/zhaojianxin/pen/rrxyXy)

**html**

```
<ul>
	<li>111111</li>
	<li>222222</li>
	<li>333333</li>
	<li>444444</li>
	<li>555555</li>
	<li>666666</li>
	<li>777777</li>
</ul>

```

**js**

```	
// 获取ul
var $ulDom = $("ul");

//给ul绑定click事件
$ulDom.on("click","li",function(){
	alert($(this).html())
});

```
