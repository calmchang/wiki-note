
# 正则表达式

### 设想我们是否遇到这些问题？
一个url的参数列表  
var str= "?am_id=2&approach=test&type=2&phone=137000";
* 取出type的值？
* 替换type的值？
* 直接删除type这个参数

一个替换字符串的案例  
var str = "{param1} name is {param2}";
* 将{param1}替换为my，将{param2}替换为changxu ，结果变为了 my name is changxu
* 将符合规则“{参数名}”格式的字符串，整个替换掉，替换的目标字符串根据"参数名"。
  
你们会怎么做？

### js中正则的使用

* /正则表达式/.test("字符串");
* var patt = new RegExp("正则表达式","扩展参数");  
  patt.test("字符串")


### match和exec的区别

#### 使用方法不一样
		
		* match是字符串包装对象的方法，用法：String.match(RegExp);
		* exec是正则表达式对象的方法，用法：RegExp.exec(String);


#### 举个例子：在字符串中查找出所有匹配规则的字符串

```
	var str = "changxu is chang + xu , changxu is a boy";
	var patten = /changxu/; //寻找出所有的changxu来
	var result = patten.exec(str);

	console.log(result);
	console.log(patten.lastIndex);
```
```
	打印结果:
	["changxu", index: 0, input: "changxu is chang + xu , changxu is a boy"]
	0
	
	总结：
	* 只找出了一次，但是我们想要找出所有的，怎么办？
	* result为一个array结构，可以通过result.length 查询长度。
	* result.input：为当前执行搜索的字符串原型,由于基本没啥用处，所以下面的案例里面我省略了这个参数
	* result.index：代表当前关键字出现的第一个位置

```


### 利用g参数来循环查找我们需要的字符止到结尾
```
	var str = "changxu is chang + xu , changxu is a boy";
	var patten = /changxu/g; //寻找出所有的changxu来
	var result = patten.exec(str);
	while( patten.lastIndex != 0  ){
		result = patten.exec(str);
		console.log(result);
		console.log(patten.lastIndex);
	}
```
```
	结果：
	["changxu", index: 0]
	7
	["changxu", index: 24]
	31
	0
```

### 我们将所有的changxu替换为god
```
	var str = "changxu is chang + xu , changxu is a boy";
	var patten = /changxu/g; //如果没g的情况下，只会替换一次
	var result = str.replace(patten,"god");
	console.log(result);
	console.log(patten.lastIndex);
```
```
	结果：
	god is chang + xu , god is a boy
	0
```

### 我们以changxu为目标，切割字符串
```
	var str = "changxu is chang + xu , changxu is a boy";
	var patten = /changxu/; // 这里有g和无g没区别
	var result = str.split(patten);
	console.log(result);
	console.log(patten.lastIndex);
```
```
	结果：
	["", " is chang + xu , ", " is a boy"]
	0
```

### ()的用法
举个例子：我们搜索一个字符串内所有符合在{}之间的字符串，并且需要把中间的内容取出来
```
	var str = "{i love you},{i need you},{you love me},you need me";
	var patten = /{i (\w+) you}/g; 
	var result = patten.exec(str);
	console.log(result);
	console.log(patten.lastIndex);
	while( patten.lastIndex != 0  ){
    result = patten.exec(str);
    if( result ) console.log(result);
    console.log(patten.lastIndex);
	}
```
```
	结果：
	["{i love you}", "love", index: 0]
	12
	["{i need you}", "need", index: 13]
	25
	0
```

### 那么现在如何将这些找出来的字符替换成我们想要的呢？

还是这个例子，我们{ i @key you} 里面的这个@key找出来了，那么将@key全部替换为“fuck”怎么做呢？

```
	var str = "{i love you},{i need you},{you love me},you need me";
	var patten = /{i (\w+) you}/g; 
	var result = str.replace( patten, function(allKey,key1){//如果有多个key那么这里可以增加key2,key3,key4....
	      console.log(allKey+'\t'+key1);
        allKey = allKey.replace(key1,"fuck");
        return allKey;
    } );

	console.log(result);
```
```
	结果：
	{i love you} love
	{i need you} need
	{i fuck you},{i fuck you},{you love me},you need me
```

### i参数忽略大小写
	var patten = /{i (\w+) you}/ig; 




