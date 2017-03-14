
# js的get、set和Object.defineProperty的应用

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

