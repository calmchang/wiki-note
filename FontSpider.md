

## font-spider 字体生成器

font-spider 原理是由于字体文件一般都很大（因为包含所有字符的信息）而我们页面内可能只用到部分文字，所以font-spider将字体文件内我们用到的字符信息提取出来，打包成新的字体文件，这样字体文件的大小就很小了。


* 安装nodejs
* 安装font-spider `npm install font-spider -g`
* 编写一个html，页面内的内容如下，目的是将所有用到的文字用我们想用的字体绘制一遍

```
<!DOCTYPE html>
<html>
<head>

	<style>
			@font-face
			{
			   font-family:diyFont;
			   src:url('JFRingmaster.ttf'); //JFRingmaster.ttf 为当前需要压缩的字体文件
			}
	</style>

</head>
	
<body>
	<p style="font-family: diyFont; font-size: 20px;">ChangXu这里把所有用到的文字输入在这里</p>
</body>
</html>

```

* 然后用font-spider来生成新的字体文件
	* `font-spider index.html`  
	* 执行完毕后会自动覆盖原来的ttf字体文件，然后拿这个字体文件去用就行啦

* 也可以利用gulp进行打包,详细见 `http://font-spider.org/`