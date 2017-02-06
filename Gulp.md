
## Gulp使用帮助及插件大全

* CSS相关
	* gulp-sass
	* gulp-minify-css
	* gulp-autoprefixer

* JS相关
	* gulp-uglify
	* gulp-babel ：用于支持ES6语法，同时需要安装 `babel-core` 和 `babel-preset-es2015`
	* gulp-sourcemaps ： 用于生成js的sourcemap方便上线后问题的调试
		```
		gulp.src([ '*.js' ] )
	    .pipe(  sourcemaps.init({loadMaps:false}) )
	    //这里可以进行压缩 混淆等等
	    .pipe( 
	      sourcemaps.write('../maps',{  //指定sourcemap文件生成到哪个目录去
		      sourceMappingURL:function(file){//这里意思是告诉浏览器sourcemap文件存放在哪里，以便浏览器自动打开
		        return 'https://m.sit.nonobank.com/'+options.template_data_list.root+'/maps/'+file.relative+'.map';
		      }
	    	})
	    )
	    .pipe(gulp.dest( "dist" );
		```
	* gulp-rev  ： gulp-rev用来按照文件的MD5重新命名文件名输出
		```
		gulp.task('_rev_create_', function(){
			return gulp.src( ['.tmp/**/*','!.tmp/*.html'] )
				.pipe(rev()) //根据文件MD5修改文件名输出到.tmp/dist下
				.pipe(gulp.dest( '.tmp/dist' ))
				.pipe(rev.manifest())  //生成替换的文件信息文件输出到tmp/rev下
				.pipe(gulp.dest( '.tmp/rev' ));

		});
		```
	* gulp-rev-collector ： 根据gulp-rev生成的文件名列表，替换所有引用到了这些文件的地方
		```
		gulp.task('_rev_replace_', function(){
			return gulp.src( ['.tmp/rev/rev-manifest.json','.tmp/dist/**/*','!.tmp/dist/resource/**/*'] )
					.pipe(revCollector({
						replaceReved: true,
					})).pipe(gulp.dest( 'dist' ));
		});
		```
	* gulp-manifest ： 生成manifest文件
		```
		gulp.task('_manifest_',function(){
			return gulp.src( ['dist/**/*'] )
			.pipe(manifest({
		      hash: true,
		      preferOnline: true,
		      network: ['*'],
		      filename: 'app.manifest',
		      exclude: 'app.manifest'
		     }))
			.pipe(gulp.dest( 'dist' ));

		});
		```

* HTML相关
	* gulp-file-include

* css js html通用
	* gulp-template


* 打包相关
	* gulp-concat
	* gulp-rename
	* gulp-sequence

* WEB服务器
	* gulp-connect