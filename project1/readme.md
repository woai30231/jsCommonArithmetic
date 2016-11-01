### 数组的排序

* 普通的数组，我们可以利用sort的方式排序来进行排序(升序)，如：

``` javascript
	
	var arr = [1,6,3,5,7];
	arr.sort();
	//输出 [1,3,5,6,7]

```

可是我们发现，实际使用过程中，sort的作用很局限，或者说是有问题的，再看下面的例子：

``` javascript

	var arr = [11,2];
	arr.sort();
	//输出[11,2]

```

数组并没有按照意愿排序（升序），究竟什么原因呢！其实sort默认排序的方式是按字符的方式排序的，11和2相比，它们从左边开始的第一个字符分别是1和2，很明显1是在2前面的，所以就造成了11排在2后面了！

不过还好，sort方法接受回调函数callback，使用callback我们就可以实现更加准确的排序了，如：

``` javascript
	
	var arr = [11,2];
	function compare(value1,value2){
		return value1 - value2;
	};
	arr.sort();
	//输出[2,11]

```


当然了现实中的数据结构更多地是由对象组成的数组，如：

``` javascript

	var arr = [
		{
			name:'xiaoming',
			age:28,
			color:"red",
			id:2
		},
		{
			name:'lisi',
			age:34,
			color:'green',
			id:1
		}
	];

```

我们同样可以采取sort方法回调的方式来实现按某个属性顺序的排序，如：

``` javascript

	function compare(_property){
		return function(obj1,obj2){
			var value1 = obj1[_property];
			var value2 = obj2[_property];
			return value1 - value2;
		};
	};
	arr.sort(compare('id'));
	//输出按id升序的对象数组

```

