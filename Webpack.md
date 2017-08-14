
## Webpack使用帮助及插件大全
* 注意事项
	* loaders加载时，执行顺序是从右往左的  
	例如 `{test: /\.scss$/, loader: 'style-loader!css-loader!postcss-loader!sass-loader'}` 加载的时候会先执行sass-loader，最后执行style-loader



* CSS相关
	* 实现scss文件编译、并且利用autoprefixer插件自动增加CSS兼容头部
		* 需要插件 require('autoprefixer');
		* 在根目录创建配置文件postcss.config.js 配置内容

```javascript
module.exports = {
  plugins: [
    require('autoprefixer')({browsers: ['last 5 version']})
  ]
};
```

	* webpack.config.js 加载器部分配置如下

```javascript
module:{
	loaders:[
		{test: /\.scss$/, loader: 'style-loader!css-loader!postcss-loader!sass-loader'},
	]
}
```

## html-webpack-plugin 插件
* 参数说明:
  title: 设置title的名字   
  filename: 设置这个html的文件名   
  template:要使用的模块的路径  
  inject: 把模板注入到哪个标签后 'body',   
  favicon: 给html添加一个favicon  './images/favico.ico',   
  minify:是否压缩  {...} | false （最新api变动，原来是ture|false 感谢@onmi指正)
  hash:是否hash化 true false ,     
  cache:是否缓存,   
  showErrors:是否显示错误,  
  chunks:目前没太明白  
  xhtml:是否自动毕业标签 默认false  