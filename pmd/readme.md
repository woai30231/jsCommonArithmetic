## 概览

* 这个测试代码是采用过程式的方式编写出来的，并没有按对象式的方式写js代码。

* 循环机制采用了setTimeout代替setInterval，用于提升性能。关于为什么，可以点击[这里](https://github.com/woai30231/javascriptThreadStudy)查看原因。

* 代码采用了一些机制避免变量名字全局污染，例如采用即时执行函数。

* 代码的细粒化，很多功能并不是写在一个超级大的函数里面，这样写晦涩难懂，并且容易头绪复杂，函数耦合大，后期难以维护，所以代码中尽量把相关代码构造成一些小的的函数。通过在大函数里面调用小函数实现大功能。

## 查看效果

* 把项目copy到本地浏览index.html

## 附上源码

``` javascript

	function addEvent(dom,type,handler){
		return dom.addEventListener?dom.addEventListener(type,handler,false):dom.attachEvent("on"+type,handler);
	};
	(function(){
		function _fn(){
			var dom = document.getElementById('nav');
			if(dom){
				var parentDom = dom.parentNode;
				var width = parentDom.offsetWidth;
				width = width - 30;
				dom.style.width = width + 'px';
			};
		};
		//默认执行一次
		_fn();
		addEvent(window,'resize',_fn);
	})();

	(function(){
		var dom = document.getElementById('nav');
		if(!dom)return;
		var domParent = dom.parentNode;
		var isStopLoop = false;
		var isToTop = document.querySelector(".switch_btn > .btn.top");
		var isToBottom = document.querySelector('.switch_btn > .btn.bottom');
		var top = 0;//初始高度
		var domChildren = dom.getElementsByTagName('li');
		var cLength = domChildren.length;
		var loopArr = (function(){
			var arr = [];
			for(var i = 0;i<cLength;i++){
				arr.push(i);
			};
			return arr;
		})();
		var loopHeight = domChildren[0].offsetHeight;
		//是否正在执行动画
		var isOnAnimation = false;
		var clearTimeoutId;
		var loopHanlder = function _fn(){
			if(isStopLoop)return;
			isOnAnimation = true;
			loopArr.push(loopArr.shift());
			setPos();
			clearTimeoutId = setTimeout(_fn,3000);
		};
		//定义一个函数实现导航list去样式
		function add_class(index){
			for(var i = 0;i<cLength;i++)(function(n){
				var cTxt = domChildren[n].className.replace(/^\s+|\s+$/g,'').split(/\s+/);
				var num = cTxt.indexOf('active');
				if(num > -1){
					var cssArr = (function(){
						var arr = [];
						for(var j = 0;j<cTxt.length;j++){
							if(cTxt[j] != 'active'){
								arr.push(cTxt[j]);
							};
						};
						return arr;
					})();
					domChildren[n].className = cssArr.join(' ');
				};

			})(i);
			domChildren[index].className += ' active';
		};
		add_class(0);
		loopHanlder();
		domParent.onmouseover = function(e){
			clearTimeout(clearTimeoutId);
			if(e.stopPropagation){
				e.stopPropagation();
			};
			isStopLoop = true;
			return false;
		};
		domParent.onmouseout = function(e){
			if(e.stopPropagation){
				e.stopPropagation();
			};
			isStopLoop = false;
			loopHanlder();
			return false;
		};
		addEvent(isToTop,'click',toTopHandler);
		addEvent(isToBottom,'click',toBottomHandler);
		function toTopHandler(){
			isStopLoop = true;
			loopArr.push(loopArr.shift());
			setPos();
			clearTimeout(clearTimeoutId);
		};
		function toBottomHandler(){
			isStopLoop = true;
			loopArr.unshift(loopArr.pop());
			setPos();
			clearTimeout(clearTimeoutId);
		};
		function setPos(){
			top = - loopArr[0] * loopHeight;
			add_class(loopArr[0]);
			dom.style.top = top + 'px';
		};


	})();	

```