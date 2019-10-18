[TOC]
## 如果您喜欢我写的内容，那么给我一个星星吧！※


### 数组遍历方法的区别

![](MobileDeveloper/arraymap.png)

* 除了for是万精油，性能也最好外，其实其他几种方法都是针对不同场景下的，所以不用太纠结性能问题，只要选择适合自己的场景就行，杠精的话可以用for实现一切

### select标签
  * 获取选项值的时候 dom.selectedOptions[0]不兼容，应该用dom.options[dom.selectIndex]获取到对象

### img标签
  * src为空的时候，display设置为none
  * 标签内最好添加 onerror=''  错误监听，不然可能引发全局异常

### a标签
	* 打电话及发短信实现（打电话需要<meta name="format-detection" content="telephone=yes">将电话功能打开)
	* <a href="sms:10086">发短信给: 10086</a> 
	* <a href="tel:020-11811922">打电话给:0755-10086</a>

### input标签
  * 在禁止用户编辑的时候，设置readonly="readonly",不要用disabled
  * type为file的时候，如果display设置为none会导致某些机器无法正常使用，所以通过透明度来隐藏<div style="opacity:0;"><input /></div>
  * android机上，当软键盘弹出后可能会遮挡输入框，解决方法使用 scrollIntoViewIfNeeded
```
   window.addEventListener("resize", function() {
    if(document.activeElement){
      if(document.activeElement.tagName=="INPUT" || document.activeElement.tagName=="TEXTAREA") {
        if(document.activeElement.scrollIntoViewIfNeeded){
          window.setTimeout(function() {
            document.activeElement.scrollIntoViewIfNeeded();
          },0);
       }
      }
    }
  });
```
	* 用户体验的优化：
		* 获取输入内容时，用"".trim()去除首尾的空格
		* 如果要求任何空格或不可见字符不能出现在字符串内，则可以利用正则 "xxx".replace(/\s/g,"");//将不可见字符删除，提高准确率

	* webapp调用照片摄像头等  
```

        <input type="file" accept="image/*" capture="camera">//必须拍照
        <input type="file" accept="video/*" capture="camcorder">//必须录像
        <input type="file" accept="audio/*" capture="microphone">//必须拍照或录像

        <input id="fileImage" type="file" accept="image/*">//获取图片
        <input id="fileImage" type="file" accept="video/*">//获取录像
        <input id="fileImage" type="file" accept="audio/*">//获取图片或录像
```

### jsBrdge
  * (待验证)在安卓上通过JsBridge来传输BASE64字符串的话，需要UrlEncode两次后回调，不然会报异常

### 时间
  * 在IOS内使用new Date('12-01-02 12:02:02') 引发异常，应该使用 new Date('12/01/02 12:02:02') 或 new Date(时间戳)

### sessionStorage
  * 安卓手机QQ浏览器：在页面启动时（快速调用）sessionStorage.getItem( 'testSession' )
那么切换页面后sessionStorage会被清空
解决方法用window.name代替
```javascript
测试代码：
<script>

sessionStorage.getItem( 'testSession' );
window.onload = function(){

	document.getElementById('btnNext').onclick = function(e)
	{
			alert('ver5');
			alert(window.name);
			window.name = new Date().getTime();
			alert('写入时间:'+window.name);
			sessionStorage.setItem('testSession',"测试55678");

			location.href = 'testSession2.html?ver=2';
			return;


	};

};
</script>
```

### meta标签
  * <meta name="renderer" content="webkit">  开启其他浏览器高速模式
  * <meta http-equiv="X-UA-Compatible" content="IE=edge"> 让IE启用新的渲染模式
  * 非强制分辨率模式：<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1, maximum-scale=1,user-scalable=no">
  * 强制分辨率模式：
```javascript
<script>
var deviceWidth =  parseInt(window.screen.width);
var destWidth = 640;
var deviceScale = deviceWidth/destWidth;
var ua = navigator.userAgent;
if (/Android (\d+\.\d+)/.test(ua))
{
	var version = parseFloat(RegExp.$1);
	if(version>2.3){}
	else
	{
		deviceScale = destWidth/1000;
	}
	document.write('<meta name="viewport" content="width='+destWidth+',initial-scale='+deviceScale+', minimum-scale = '+deviceScale+', maximum-scale = '+deviceScale+', target-densitydpi=device-dpi">');
} 
else
{
	document.write('<meta name="viewport" content="width='+destWidth+', user-scalable=no">');
}
</script>
```

### 元素尺寸
  * 获取一个标签的真实宽度高度(比如<p>标签文字真实长度) dom.scrollWidth ,而获取一个标签的可视宽高度为 dom.clientWidth;
  * 获取屏幕的可视高度 window.screen.height*window.devicePixelRatio


### 优化
  * 在往DOM频繁或者插入大量元素的操作时，申请一个临时空间document.createDocumentFragment()，放入临时空间完成后一起加入到DOM

### SASS
  * 计算的值为%值时用percentage函数
```javascript
span {  
    width:percentage(2/3);  
} 
结果为
span {  
    width:66.66667%;  
}
```

  * 自动生成CSS并且CSS名字也是动态计算出来，#{$name}  替换生成名字的话需要#{}
```javascript
@mixin locate($name,$size:1)
{
        @keyframes #{$name}
	{
		0%{transform: scale(0);}
		100%{transform: scale($size);}
	}
}
```

### CSS
```
/*
 * 参数介绍
 * 中心定位XY坐标可能性:center/left bottom/10% 10%/12px 12px/
 * 圆的类型:contain/circle/ellipse  farthest-corner/closest-side
 * 可变长的颜色过渡列表:颜色1 所占百分比,颜色2 所占百分比....
 */
background: -webkit-radial-gradient(50% 50%,circle,rgba(255,255,255,1),rgba(255,255,255,.8),rgba(255,255,255,0),rgba(255,255,255,0));


	/*重复圆形渐变*/
	/*
	background:-webkit-repeating-radial-gradient(#ace, #ace 5px, #f96 5px, #f96 10px);
	*/
	
	/*重复线形渐变
	 * top 是从上到下、left 是从左到右，
	 */
	/*background:-webkit-repeating-linear-gradient(left, #ff0000,transparent );
	-webkit-background-size: 50px 50px;*/
	 background-color: #0ae;
	 background-image: -webkit-gradient(linear, 0 0, 0 100%, color-stop(.5, rgba(255, 255, 255, .2)), color-stop(.5, transparent), to(transparent));
```

### 滚动条
设置id为box的dom滚动条样式，要求在内容超出的时候显示滚动条，内容不超出的时候隐藏滚动条

CSS样式设置如下:

```
#box{
	overflow-y:auto;//如果需要一直显示则改为scroll
}

//接下来定义滚动条的样式
#box::-webkit-scrollbar-track
{
  -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
  border-radius: 10px;
  background-color: #F5F5F5;
}

#box::-webkit-scrollbar
{
  width: 12px;
  background-color: #F5F5F5;
}

#box::-webkit-scrollbar-thumb
{
  border-radius: 10px;
  -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);
  background-color: #D62929;
}
```

### sass

父选择器  
  源代码
```
    a{ &:hover{color:red}} 
```
  编译后
```
    a{ a:hover{color:red}} 
```

占位符选择器  
  源代码
```
  %big{font-size:100px} 
  .style1{
    @extend %big;
  }
  .style2{
    @extend %big;
  }
```
  编译后
```
  .style1,.style2{
    font-size:100px
  }
```
  如果%big没有被使用到，编译结果内将不存在

@mixin和@include，mixin代码段将替换到@include处
  源代码
```
  @mixin big{ font-size:100px}
  .style1{@include big;}
  .style2{@include big;}
```
  编译后
```
  .style1{font-size:100px}
  .style2{font-size:100px}
```

@function 函数
  源代码
```
  @function big(){@return 100+px;}
  .style1{font-size:big();}
```
  编译后
```
  .style1{font-size:100px;}
```


div + p: 代表 div 同级下相邻的第一个 p 元素
```
    <div>
      <p>不满足</p>
    </div>
    <p>满足</p>
    <p>不满足</p>
```

div > p : 代表在div内部所有p标签的子元素 
```
  <div>
    <p>满足</p>
    <hgroup>
      <p>不满足</p>
    </hgroup>
    <p>满足</p>
  </div>
  <p>不满足</p>
```



### ajax网络请求
  * 在调用ajax请求时，msg为字符串和msg为object，结果不同，代码如下：  
```javascript
var inter = "https://m.pre.nonobank.com/nono-web/weixinLoan/login";

var msg = 'image=data%3aimage%2fpng%3bbase64%2c';// image内为一个进行了urlEncode的BASE64图片代码
var settings =
{
 	type: "POST",
	data: msg,
	async: true,
	url:inter,
};
$.ajax(settings);
//在chrome内查看消息参数时，被解析为 image:data:image/png;base64

var msg2 =  {image:"data%3aimage%2fpng%3bbase64%2c" };
var settings2 =
{
 	type: "POST",
	data: msg2,
	async: true,
	url:inter,
};
$.ajax(settings2);
//在chrome内查看消息参数时，被解析为 image:data%3aimage%2fpng%3bbase64%2c

```
  * 可见在以对象传递给msg的时候，不会对里面的参数进行urldecode，而以string传递的时候则会进行一次urldecode
  * 所以在服务端收到消息的时候，建议都进行一次urldecode比较安全


### Chrome的插件和常用功能
	* 当我们变更过DNS后，强制清除CHROME的缓存方法
		* 地址栏打开 `chrome://net-internals/#capture` 点击右上角箭头
		* 分别执行 clear cache 和 flush sockets
	* 常用插件
		* CORS Toggle ： 允许跨域（注意需要设置header内允许的字段）
		* Ad Block ： 阻止弹出广告
		* FireShot ： 可以给网页截图（截取后下载到本地）
		* 网页截图:注释&批注 ： www.awesomescreenshot.com提供的，也是用来网页截图的，截图后还可以进行在线简单编辑后下载本地
		* Github Toc ： 让GITHUB内支持markdown的toc目录标签



### encodeURI、encodeURIComponent、escape、RFC3986编码的区别

* 关于以上4种编码的区别，我用以下图来进行对比，表格上为4种编码对各个字符编码后的结果
* 总结：编码最完全，最安全的应该是RFC3986编码，我们常用的编码URL后面参数用的是:encodeURIComponent，encodeURI和escape最不常用

![](MobileDeveloper/encode.jpg)
 

### manifest使用方法  
* 创建 application.appcache 配置文件
	
~~~javascript
		CACHE MANIFEST
		#2015-5-20-18-02 这一行用来代表版本号
		testcache.html
~~~

* 头部定义缓存配置文件 `<html manifest='application.appcache'>`  
* 在页面内增加监听方法,目的是检测到缓存配置文件发生变化的话就重新载入页面  
~~~javascript
		window.applicationCache.onupdateready=function(e)
		{
			console.log('update cache');
			location.reload();
		};
~~~
	
### ES5中继承的方法
* 简单期间，封装了一个用于继承2个类的方法如下:

~~~javascript

	function ext(son,father){
		//遍历当期那子类的所有方法，如果出现重复，则不用父类覆盖子类
		for( var param in father.prototype ){
			if( !son.__proto__[param ]){
				son.__proto__[param] = father.prototype[param];
			}else{
				son.__proto__[param].super = father.prototype[param];//【可选】将父类方法预留到super内，以备调用
			}
		}
		//先调用父类构造方法进行初始化
		father.apply(son,arguments[2]);
		//创建runSuper方法，用于调用父类的同名方法【可选】
		son.runSuper = function(funName,args){
			if(this.__proto__[funName].super)this.__proto__[funName].super.apply(this,args);
		}.bind(son);
	}
	
	//使用方法如下
	function Father(name){
		this.name= name;
	}
	Father.prototype.age = function(age){
		console.log('father name is ' + this.name +' ,age is ' + age);
	};

	function Son(name){
		ext(this,Father,arguments);
	}
	Son.prototype.age = function(age){
		this.runSuper('age',arguments);//【可选】运行父类方法
		console.log('son name is ' + this.name +' ,age is ' + age);
	};
	var son = new Son("cx");
	son.age(23);
	//output:
	//father name is cx ,age is 23
	//son name is cx ,age is 23

~~~

### MAC OS开发常用软件
* git: SourceTree
* svn: Cornerstone  `http://www.zennaware.com/cornerstone/`
* 抓包: Charles
* git 对比: DiffMerge
* 屏幕录像: LICEcap
* 远程服务器: zoc7
* 文件复制帮手: Yoink
* 压缩解压: The Unarchiver
* 代理软件: ShadowsocksX


### ajax相关
* 在zepto内默认的Content-Type为application/x-www-form-urlencoded
  
  当data为一个object时，zepto将会把它转换成key1=value1&key2=value2的形式，转换过程中，会对key和value进行urlencode。

  所以在服务器拿到消息时，会默认进行一次urldecode

  而如果data为一个字符串时，则zepto不会对其进行urlencode


### WEB加载的大致流程 - window.onload、DOMContentLoaded、load 之间的区别

* 我在这边画了一张流程图来展示整个url打开的过程  

![](MobileDeveloper/timeline.png)

* 总结一下
	* 1、几个load事件调用先后顺序 `window.onload` -> `DOMContentLoaded` -> `load`
	* 2、其中`DOMContentLoaded`代表已经首屏展示（如果你没有强制设置隐藏的话）、load事件代表全部非异步加载的资源全部就绪了。
	* 提高页面加载速度的技巧：
		* 减少非首屏必须的js和css加载，可以放入load后异步加载
		* `load`事件会等待页面所有资源加载完毕后才触发，所以类似img标签的图片资源加载过程是不影响`DOMContentLoaded`事件的触发的，但是只有当img全部加载完毕才会触发load,优化方法是img src使用默认一张统一的图片，然后在`load`事件后异步加载.

### 其他
* Object.assign在安卓浏览器上存在兼容问题,解决方案如下：

```javascript
//注意这里是非深度拷贝

if (typeof Object.assign != 'function') {
  // Must be writable: true, enumerable: false, configurable: true
  Object.defineProperty(Object, "assign", {
    value: function assign(target, varArgs) { // .length of function is 2
      'use strict';
      if (target == null) { // TypeError if undefined or null
        throw new TypeError('Cannot convert undefined or null to object');
      }

      var to = Object(target);

      for (var index = 1; index < arguments.length; index++) {
        var nextSource = arguments[index];

        if (nextSource != null) { // Skip over if undefined or null
          for (var nextKey in nextSource) {
            // Avoid bugs when hasOwnProperty is shadowed
            if (Object.prototype.hasOwnProperty.call(nextSource, nextKey)) {
              to[nextKey] = nextSource[nextKey];
            }
          }
        }
      }
      return to;
    },
    writable: true,
    configurable: true
  });
}
```

### CSS动画
	
	* 对transform增加过度动画： dom.style.transition="-webkit-transform .5s ease-in";
	* 动态修改transform3d: dom.style.webkitTransform="translate3d(0,0,0)";
	

### ES6

#### 变量申明

* const 声明常量，声明后不可改变值  
* let 声明变量，但是let没有提升变量效果，要注意声明的位置，只有在声明位置后才能被正确调用

#### 字符串模板

利用模板字符串来组合字符串更方便
```
 let name="cx";
 let ret = `我的名字叫 ${name}`; 
```

#### 函数

* 默认参数代替原来a=a||100, `function test(a=100,b=200){}`  
* 免写function和冒号 test(){}
* 箭头函数  
	* 原先使用 fun.bind(obj)()或fun.apply(obj);的地方，可以用箭头函数代替
	* 缺点：不能改变this对象、没有arguments、不存在prototype所以不能被new无法继承  
  
* apply、bind、call区别
  * apply和call 传参方式不同
    * fn.call(this,a,b,c);
    * fn.apply(this,[a,b,c]);
    * fn.bind(this,a,b,c);
  * bind用于生成新对象,apply和call立即执行，当需要生成一个新对象一直绑定某个obj的时候，使用bind
    * var myFn=fn.bind(this,a,b,c); myFn();


#### 对象
* 对象免键值  

```
  var name=‘cx’,age=12,nation="china";
  var p={
  	name,
  	age,
  	nation
  };//等同于{name:name,age:age,nation:nation}
```

* 获取对象中某些键值  
  let {name,age} = p;//等同于 let name=p.name,age=p.age;

* 展开运算符...  

```
//合并2个对象，可用于代替 Object.assign({},obj,obj2);
var obj={a:1,b:2,c:3};
var obj2={c:4}; 
var obj3={...obj,...obj2}; // obj3={a:1,b:2,c:4};

var arr=[1,2,3];
var arr2=[4,5,6];
var arr3=[…arr,…arr2];// arr3=[1,2,3,4,5,6];

```

* 在函数中要习惯用展开运算符代替arguments  

```
function add(...nums){
	let ret=0;
	for( let i=0;i<nums.length;i++){
		ret+=nums[i];
	}
	return ret;
}
var num=[1,2,3];
add(num);
```


#### import/export  
* 两种export的方法对应的import的使用：  
	1、 导出为一个集合 
		
		export {App}  
		import的时候： import {App} from 'dir/filename'; 

		当有多个对象的时候export {App,App2}   
		import的时候： import {App,App2} from 'dir/filename';   
		也可以重命名 import {App as YourDiyName,App2 as YourDiyName2} from 'dir/filename';   

		
	2、 默认导出 - 默认导出只能导出1个对象  
		export default App;  
		import的时候： import YourDiyName from 'dir/filename';

* 查询字符串内是否包含某个字符串 : 用 "abcde".includes("cd") 代替 "abcde".indexOf("cd") >= 0

* 格式化时间字符串  

```
  var date = new Date(); 
  var type ="zh-CN";
  var types=["2-digit","numeric","narrow","short","long"];

  var options = {   
    hour:types[1],
    minute:types[1],
    second:types[1],
    hour12:false,
  };


  //14:08:30
  console.log( new Intl.DateTimeFormat(type,options).format(date) );


  var options = {   
  	year:types[1],
  	month: types[0],
    day: types[0],
    hour:types[1],
    minute:types[1],
    second:types[1],
    hour12:false
  };

  //2018/05/22 14:06:59
  console.log( new Intl.DateTimeFormat(type,options).format(date) );


  var options = {   
  	year:types[1],
  	month: types[2],
    day: types[0],

  };

  //2018年5月22日
  console.log( new Intl.DateTimeFormat(type,options).format(date) );


  var options = {   
  	year:types[1],
  	month: types[0],
    day: types[0],

  };

  //2018/05/22
  console.log( new Intl.DateTimeFormat(type,options).format(date) );

```

* 格式化货币: new Intl.NumberFormat(["zh-CN"],{maximumFractionDigits:2}).format(123456.1264);//"123,456.13"

### 字体兼容

* 根据个人经验目前比较省事儿的字体兼容各屏幕的方法如下
	1、UI定义出整个站点基础字号，默认移动端一般为12px  
	2、UI定义出375的屏幕横向占有多少个12px的字，假设规范定义为24个字  
	3、设置所有DOM默认字体 *{ font-size:1rem; }  
	4、将html,body{font-size:12px;}  
	5、页面加载过程中添加JS代码  
	```
		let lineCount = 375/12;
		let defaultFontSize = Math.floor(window.screen.width/lineCount;
		defaultFontSize = defaultFontSize > 12 ? 12 : defaultFontSize;
		document.getElementsByTagName('html')[0].style.fontSize=defaultFontSize+'px';
		document.getElementsByTagName('body')[0].style.fontSize=defaultFontSize+'px';
	```



### vue-cli

* vue-cli默认页面内的资源引用全部会打包成 `/static/xxx.png` 也就是去域名根目录下的static下拿  
	解决方法：修改 config/index.js内的assetsPublicPaths属性，将`/`改为`./`

* sass内background-image 引用图片打包后路径错误  
	解决方法：`build/utils.js`内修改`vue-style-loader`增加` publicPath:'../../' `的配置

* 查看当前vue-cli版本号:`vue -V`

* 使用@keyframes写动画，不要使用@-webkit-keyframes写动画


### 环境配置

* 查看当前安装的vue-cli版本号:`vue -V`
* 查看当前安装的node版本号:`node -v`
* 查看当前安装的webpack版本号:`npm ls webpack`
	* 查看本地安装的包的版本号及依赖包版本号:`npm ls 包名`
* 查看线上最新包的版本号：`npm view 包名 version`


### get、set和Object.defineProperty的应用

```
var gName={
	get:function(){
		return this._name;
	},
	set:function(value){
		this._name=value;
	}
};

gName='default';//必须设置默认值，不然下一步获得到的gName是一个object
console.log(gName);//default
gName='peter';
console.log(gName);//peter
```

```

function Per(){

	this._name='default';

	Object.defineProperty(this, 'name', {
		get:function(){return this._name;},
		set:function(value){ this._name= value;}
	});
}


```

## 微信开发相关

### 在微信内分享的url拼接注意事项

通过#来做页面路由跳转的单页网站，在微信上授权时需要注意，将?后参数放在#前

举例说明：

~~~

页面：https://test.com/#/?inviteCode=123&approach=1234

经过微信授权后，微信回调回来URL变为
https://test.con/?code=01117NaK17GH750u3DaK1iExaK117Na-&state=ws#/?inviteCode=123&approach=123

此时 vueRouter解析query为 inviteCode和approach，#之前的参数丢失

而如果页面参数为:https://test.com/?inviteCode=123&approach=1234#/

经过微信授权后，微信回调回来URL变为
https://test.con/?inviteCode=123&approach=123&code=01117NaK17GH750u3DaK1iExaK117Na-&state=ws#/

~~~

### UE

### input组件
* 获取用户输入内容进行校验或提交
	* 基于移动端输入法较多，以及输入法混用输入情况，为了达到减少用户输入次数，更精确的拿到用户想输入的内容，我们规则如下：
	* 利用trim()去除内容的前后空格部分
	* 非特殊情况下，使用 replace("/\s/g,"") 去除字符串内所有的空格及不可见字符
  


### 设计模式

* 工厂模式
  产生规定规格内的相似对象
  应用场景：创建组件、创建各类对象等
```
  function createPerson(name,age,sex){
    let obj=new Object();
    obj.name=name;
    obj.age=age;
    obj.sex=sex;
    return obj;
  }
```

* 观察者模式
  每次触发行为将通知所有的观察者
  应用场景：事件监听、拦截器
```
  let eatWacher=[fn,fn2,fn3]
  function eat(){
    console.log('eatting');
    eatWacher.map((fn)=>{fn()});
  }
```

* 单体模式
  对象只被实例化一次，重复调用时为已创建的实例
  应用场景：只需要创建一次对象即可的，比如全局的组件
```
  function getInstance(){
    if(!this.instance){
      this.instance=new Object();
    }
    return this.instance;
  }
```


* 订阅-发布模式
  订阅者像订阅中心预定自己关心的消息类型
  订阅中心在收到事件的时候，分发给订阅者他们关心的事务
  应用场景：消息中心
```
  var buyPhoneEvent={
    list:[],
    listen(key,fn){
      this.list[key]=this.list[key]||[];
      this.list[key].push(fn);
    },
    tigger(...params){
      var key = (params&&params[0])?params[0]:"";
      if(!key)return;
      var lst = this.list[key];
      if(!lst||lst.length===0)return;
      lst.map((fn)=>{fn.apply(this,params)})
    }
  };

  buyPhoneEvent.listen('iphone',(...params)=>{console.log(`buy iphone ${params[0]}-${params[1]}`)});
  buyPhoneEvent.listen('huawei',(...params)=>{console.log(`buy huawei ${params[0]}-${params[1]}`)});

  buyPhoneEvent.tigger("iphone","4999");
  buyPhoneEvent.tigger("huawei","2999");
  buyPhoneEvent.tigger("xiaomi","999");
```



