
## 使用jsduck来生成代码文档的方法


* Ruby Gem 环境安装
    * 参考`https://rubygems.org/pages/download`

* 安装环境 for Mac
    * 打开终端 `sudo gem install jsduck`
* 生成文档
    * 在终端下执行命令  `jsduck src/*.js -o doc`  其中`src/*.js`为需要生成的js目录 `doc` 为文档的输出目录
* jsduck文档编案例，将一下代码保存为JS文件然后用命令生成文档查看结果
```
    /**
     * @class _Earth
     * 描述我们地球的类
     */
     
     /**
      * @class _Earth.global
      * 全局变量存放的地方
      */
     
     /**
      * 记录地球上动物种类的数量
      * @type {Number}
      */
    var animalTypeCount = 20;

     /**
      * 存放所有动物
      */
    var animals ={
        dog:null,
        cat:null,
    };

    /**
     * 创建一个地球
     * @return {Object} [地球对象]
     */
    function createEarth(){
        return {};
    }

    /**
     * @class _Earth._Animal
     * 这是一个动物的类
     * @singleton
     */
    function Animal(){
        this.name = "";
    }

    /**
     * 设置动物的名字
     * @param {String} name 姓名
     */
    Animal.prototype.setName = function(name){
        this.name = name;
    };

    /**
     * 获取动物的名字
     * @param {Object} str 源字符串
     * @param {Object} key 密钥
     * @return {String} 动物的名字
     */
    Animal.prototype.getName = function(){
        return this.name;
    };

    /**
     * @class _Earth._Animal.Dog
     * 狗类
     */
    function Dog(){
        this.name = "";
    }

    /**
     * 狗狗的呼吸
     */
    Dog.prototype.breath = function(){
    };

```