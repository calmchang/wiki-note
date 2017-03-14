[TOC]

### select标签
  * 获取选项值的时候 dom.selectedOptions[0]不兼容，应该用dom.options[dom.selectIndex]获取到对象

### img标签
  * src为空的时候，display设置为none
  * 标签内最好添加 onerror=''  错误监听，不然可能引发全局异常

### input标签
  * 在禁止用户编辑的时候，设置readonly="readonly",不要用disabled
  * type为file的时候，如果display设置为none会导致某些机器无法正常使用，所以通过透明度来隐藏<div style="opacity:0;"><input /></div>

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


### 缓存  
## manifest使用方法  
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
	


		
