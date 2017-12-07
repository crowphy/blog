# JS 面向对象
## ES5
### 对象的定义
> 无序属性的集合，其属性可以包含基本值，对象或者函数。

### 理解对象
##### 属性特性：

描述属性的属性，亦称为特性（attribute），用于描述属性的各种特征

> ES 有两种属性特性：数据属性和访问器属性

##### 1. 数据属性：

* [[Configurable]]：可配置属性。表示能否通过 delete 删除从而重新定义属性，能否修改属性，或者能够把数据属性修改为访问器属性。对于直接定义在对象上的属性，默认为 true。一旦把其更改为 false 便不可再改回来，此时修改除 writable 之外的特性都会报错。
* [[Enumerable]]：可枚举属性。表示能否通过 for-in 循环遍历属性。对于直接定义在对象上的属性，默认为 true。
* [[Writable]]：可写属性。表示能否修改属性的值。对于直接定义在对象上的属性，默认为 true。
* [[Value]]：值属性。包含此属性的数据值。默认为 undefined。

> 可以通过 Object.define(obj, property, {attributes})修改特性。


	var person = {};
	Object.defineProperty(person, "name", {
	    writable: false,
	    value: "Crowphy"
	});
	console.log(person.name); //"Crowphy" 
	person.name = "Tick"; 
	console.log(person.name); //"Crowphy"

##### 2.访问器属性：

* [[Configurable]]：可配置属性。表示能否通过 delete 删除从而重新定义属性，能否修改属性，或者能够把数据属性修改为访问器属性。对于直接定义在对象上的属性，默认为 true。一旦把其更改为 false 便不可再改回来，此时修改除 writable 之外的特性都会报错。
* [[Enumerable]]：可枚举属性。表示能否通过 for-in 循环遍历属性。对于直接定义在对象上的属性，默认为 true。
* [[Get]]：读取属性时调用的函数。默认为 undefined
* [[Set]]：写入属性时调用的函数。默认为 undefined

##### Tips
> 访问器属性不能直接定义，必须通过 Object.defineProperty()定义。
> 
> getter和 setter 属性一般需要同时定义。否则，如果只定义 getter ，则无法写入；只定义 setter，则无法读取。

> Object.defineProperties(obj, {Properties})：一次性定义多个属性的特性。
> 
> Object.getOwnPropertyDescriptor(obj, property)：读取相关属性的特性。

### 创建对象
####  一般情况
一般情况下我们只需要一个空对象，可以通过以下几种方式：

1.

	var a = new Object()
2.

	 var a = {}
3.

	var a = Object.create(null)
	
### 多次创建
> 重点：组合使用构造函数模式和原型模式
#### 工厂模式

	function createPerson(name) {
		var o = new Object();
		o.name = name;
		o.sayName = function() {
			console.log(this.name);
		}
		return o;
	}
	var obj = createPerson('Crowphy');
	
> 缺点：无法知道一个对象的类型，因为都是直接调用Object构造函数创建的。

#### 构造函数
	function Person(name) {
		this.name = name;
		this.sayName = function() {
			console.log(this.name);
		}
	}
	var p1 = new Person('Crowphy');
使用 new 创建对象实际会经历以下四个阶段：

> 1. 创建一个新对象；
> 2. 将构造函数的作用域赋给新对象（将 this 指向该对象）；
> 3. 执行构造函数中的代码（为该对象添加属性）；
> 4. 返回该对象。


	console.log(p1 instanceof Object);  //true
	console.log(p1 instanceof Person);  //true
创建自定义的构造函数意味着将来可以将它的实例标识为一种特定的类型；而这正是构造函数模式胜过工厂模式的地方。

> 缺点：每个方法都要在每个 实例上重新创建一遍。

##### Tips
如果构造函数中 return 了一个对象，则会覆盖默认返回的对象
#### 原型模式

	function Person(){
    }
    Person.prototype.name = "Nicholas";
    Person.prototype.age = 29;
    Person.prototype.job = "Software Engineer";
    Person.prototype.sayName = function(){
        alert(this.name);
    };
    var person1 = new Person();
    person1.sayName();   //"Nicholas"
    var person2 = new Person();
    person2.sayName(); //"Nicholas"
    alert(person1.sayName == person2.sayName);  //true
    
> 缺点：
> 
> 1. 它省略了为构造函数传递初始化参数这一环节，结果所有实例在 默认情况下都将取得相同的属性值。
> 
> 2. 原型中所有属性是被很多实例共享的。

#### 组合使用构造函数模式和原型模式
		
	function Person(name, age, job){
	this.name = name; 3 this.age = age;
	this.job = job;
	this.friends = ["Shelby", "Court"];
	}
	Person.prototype = {
	    constructor : Person,
	    sayName : function(){
	        alert(this.name);
	    }
	}
	var person1 = new Person("Nicholas", 29, "Software Engineer");
	var person2 = new Person("Greg", 27, "Doctor");
	person1.friends.push("Van");
	alert(person1.friends);    //"Shelby,Count,Van"
	alert(person2.friends);    //"Shelby,Count"
	alert(person1.friends === person2.friends);//false
	alert(person1.sayName === person2.sayName);//true
	
最常用的一种创建自定义类型方法。
#### 动态原型模式

	function Person(name, age, job){
		//属性
		this.name = name; 
		this.age = age; 
		this.job = job;
		//方法
		if (typeof this.sayName != "function") {
		    Person.prototype.sayName = function(){
		        alert(this.name);
			}; 
		}
	}
通过判断方法存在与否以保证每个方法只创建一次。
	
#### 寄生构造函数模式

	function Person(name, age, job){
	    var o = new Object();
	    o.name = name;
	    o.age = age;
	    o.job = job;
	    o.sayName = function(){
	        alert(this.name);
	    };
		return o; 
	}
    var friend = new Person("Nicholas", 29, "Software Engineer");
    friend.sayName();  //"Nicholas"
与工厂模式相比，它使用了 new；与构造函数模式相比，它返回一个对象以重新默认返回的对象。
#### 稳妥构造函数模式
所谓稳妥对象，是指没有公共属性，其方法也不使用 this 对象。最适合在一些安全的环境或者防止数据被篡改的情况下使用。

	function Person(name, age, job){
	    var o = new Object();
	    o.sayName = function(){
	        alert(name);
	    };
		return o; 
	}
    var friend = Person("Nicholas", 29, "Software Engineer");
    friend.sayName();  //"Nicholas"
    
相比于寄生构造模式的不同，一不使用 new；二方法中不使用this。

### 继承
> 重点：组合继承和寄生组合式继承

#### 原型链
#### 构造函数
#### 组合继承（伪经典继承）

	```js
	function Super(name) {
		this.name = name;
	}
	Super.prototype.sayName = function() {
		console.log(this.name);
	}
	function Sub(name, age) {
		// 第一次调用父类构造函数
		Super.call(this, name);
		this.age = age;
	}
	// 第二次调用父类构造函数
	Sub.prototype = new Super();
	// 将子类原型的构造函数指回Sub
	Sub.prototype.constructor = Sub;
	```

> 优点：组合继承避免了原型链和借用构造函数的缺陷，融合了它们的优点，成为 JavaScript 中最常用的继承模式。而且，原型链保持不变，instanceof 和 isPrototypeOf()也能够用于识别基于组合继承创建的对象。

>  缺点：1.调用两次父类构造函数；2.包含多余属性（父类构造函数中的）
	
#### 原型式继承
#### 寄生式继承
#### 寄生组合式继承

	function inheritPrototype(sub, super) {
		var prototype = Object.create(super.prototype);
		prototype.constructor = sub;
		sub.prototype = prototype;
	}
	function Super(name) {
		this.name = name;
	}
	Super.prototype.sayName = function() {
		console.log(this.name);
	}
	function Sub(name, age) {
		// 第一次调用父类构造函数
		Super.call(this, name);
		this.age = age;
	}
	// 继承原型
	inheritPrototype(Sub, Super);
	
> 优点：1.只调用一次构造函数；2.不包含多余的属性；3.原型链保持不变，可以正常使用instanceof 和 isPrototypeOf()。因此是最理想的继承范式。