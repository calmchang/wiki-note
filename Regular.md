
# 正则表达式

### 设想我们是否遇到这些问题？

* 一个url的参数列表  
var str= "?am_id=2&approach=test&type=2&phone=137000";  

	* 取出type的值？
	* 替换type的值？
	* 直接删除type这个参数
	* 取出所有符合规则 参数=值 的内容

* 一个替换字符串的案例  
var str = "{param1} name is {param2}";  

	* 将{param1}替换为my，将{param2}替换为changxu ，结果变为了 my name is changxu
	* 将符合规则“{参数名}”格式的字符串，整个替换掉，替换的目标字符串根据"参数名"。
  
你们会怎么做？

### js中正则的使用

* /正则表达式/.test("字符串");
* var patt = new RegExp("正则表达式","扩展参数");  
  patt.test("字符串")


#### 常用符号

* 用于代表位置的  

| 字符 | 含义 |
|--|--|
|^|字符串开始位置,也就是第一个字符位置|
|$|字符串结尾位置,也就是最后一个字符位置|

* 用于检测匹配的表达式次数

| 字符 | 含义 |
|--|--|
|*|匹配这个符号之前的表达式，任意次数|
|+|匹配这个符号之前的表达式，至少1次|
|?|匹配这个符号之前的表达式，至少0次或者1次|
|\{n\}|匹配这个符号之前的表达式，n次|
|\{n,\}|匹配这个符号之前的表达式，至少n次|
|\{n,m\}|匹配这个符号之前的表达式，>=n && <=m 次之间|


* 组成表达式需要的参数

| 字符 | 含义 |
|--|--|
|.|匹配 \r\n 外的任意字符|
|x&#124;y|匹配x或者y任意一个，x和y可以是字符也可以是表达式|
|[xyz]|匹配括号内的任意一个字符|
|[^xyz]|匹配除了括号内的字符外的字符|
|[a-z]|匹配a-z之间的任意一个字符|
|[a-zA-Z]|匹配a-z或A-Z之间的任意一个字符|
|[0-9]|匹配0-9之间任意一个字符,等价于\d|

* 特殊作用

| 字符 | 含义 |
|--|--|
|( )|将()范围内符合表达式的结果输出到数组内，最多可以存放9组[0]-[9]|




#### 实战开始：在字符串中查找出所有匹配规则的字符串

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
	* result为一个array结构，可以通过result.length 查询长度。result[0]存放搜索结果，1~9存放了我们表达式内匹配()的部分
	* result.input：为当前执行搜索的字符串原型,由于基本没啥用处，所以下面的案例里面我省略了这个参数
	* result.index：代表当前关键字出现的第一个位置

```
##### 遗留问题：这里只查到了字符串中第一个changxu，如何查到所有的？


### 利用g参数来循环查找我们需要的字符止到结尾
```
	var str = "changxu is chang + xu , changxu is a boy";
	var patten = /changxu/g; //寻找出所有的changxu来
	var result = patten.exec(str);
	while( patten.lastIndex != 0  ){//当增加了g参数，patten.lastIndex才会起效，不然永远是0
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


### 既然找到了目标，那么我们将所有的changxu替换为god
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



### 回到最初

* 取出所有 参数=值 列表
```
var str= "?am_id=2&approach=test&type=2&phone=137000";
var result = {};
var temp = null;
var patten = /([^&?]+)=([^&?]+)/g;
temp = patten.exec(str);
if (temp) {result[temp[1]] = temp[2];}
while (patten.lastIndex !== 0) {
    temp = patten.exec(str);
    if (temp) {result[temp[1]] = temp[2];}
}
console.log(result);
```

* 查询type的值？
```
var temp = null;
var patten=/[&?]type=([^&?]+)/g;
temp = patten.exec(str);
if(temp){console.log(temp[1]);}

```



### match和exec的区别

#### 使用方法不一样
		
		* match是字符串包装对象的方法，用法：String.match(RegExp);
		* exec是正则表达式对象的方法，用法：RegExp.exec(String);





