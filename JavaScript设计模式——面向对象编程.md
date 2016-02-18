### 封装

#### prototype 简单介绍

  通过`this`添加的属性、方法是在当前对象上添加的，然而`JavaScript`是一种基于原型`prototype`的语言，所以创建一个对象时（当然在`JavaScript`中函数也是一种对象），它都有一个原型`prototype`用于指向其继承对象的属性、方法。这样通过`prototype`继承的方法并不是对象自身的，所以在使用这些方法时，需要通过`prototype`一级一级查找得到。这样你会发现通过`this`定义属性或者方法时是该对象自身拥有的，所以我们每次通过类创建一个新对象时，`this`指向的属性和方法都会得到相应的创建，而通过`prototype`继承的属性和方法是每个对象通过`prototype`访问到，所以我们每次通过类创建一个新对象时这些属性和方法不好再次创建。

#### constructor 函数属性

```javascript
var Book = function(id, bookname, price) {
  this.id = id;
  this.bookname = bookname;
  this.price = price;
}
```

`constructor`与`prototype`关系图例：

![关系图例](http://i12.tietuku.com/aa85d7096460d260.png)

  `constructor`是一个属性，当创建一个函数对象时都会为其创造一个原型对象`prototype`，在`prototype`对象中又会像函数创建`this`一样创建一个`constructor`属性，那么`constructor`属性指向的就是拥有整个原型对象的函数或者对象，参考上面的图例理解。
  
  
####  函数作用域

  由于`JavaScript`的函数级作用域，声明在函数内部的遍历已经方法在外界是访问不到的，通过次特性即可创建类的私有变量已经私有方法。然而在函数内部通过`this`创建的属性和方法，在类对象创建时，每个对象自身拥有一份且只有一份可以在外部访问到。因此通过`this`创建的属性可看成是对象共有属性和对象公有方法，而通过`this`创建的方法，不但可以访问这些共有属性和共有方法，而且还能访问到类（创建时）或者对象自身私有的属性和私有方法，由于这些方法权利比较大，所以我们又将它看做特权方法。在对象创建时通过使用这些特权方法我们可以初始化实例对象的一些属性，因此这些在创建时调用的特权方法还可以看作是类的构造器。比如下面的代码：
  
```javascript
// 私有属性与私有方法，特权方法，对象共有属性和对象共有方法，构造器
var Book = function(id, name, price) {
  // 私有属性
  var num = 1;
  // 私有方法
  function checkId() {
	
  };
  //  特权方法
  this.getName = function() {};
  this.getPrice = function() {};
  this.setName = function() {};
  this.setPrice = function() {};
  //对象的公共属性
  this.id = id;
  //对象公共方法
  this.copy = function() {};
  //  构造器
  this.setName(name);
  this.setPrice(price);
};
```

#### 闭包

  闭包是有权访问另外一个函数作用域中的变量的函数，即在一个函数内部创建另外一个函数。比如下面的代码实现：

```javascript
// 修改前面的Book类，使用闭包方式实现
var Book = (function() {
	// 静态私有变量
	var bookNum = 0;
	//  静态私有方法
	function checkBook(name) {};
	
	// 创建类
	function book(newId, newName, newPrice) {
		//私有变量
		var name, price;
		//私有方法
		function checkID(id) {};
		//特权方法
		this.getName = function() {};
		this.getPrice = function() {};
		this.setName = function() {};
		this.setPrice = function() {};
		
		//共有属性和方法
		this.id = newId;
		this.copy = function() {};
		
		bookNum++;
		
		if (bookNum > 100)
			throw new Error("我们仅仅提供100本书。");
			
		//构造器
		this.setName(name);
		this.setPrice(price);
	};
	
	//构造原型
	_book.prototype = {
		//静态共有属性
		isJSBook: false,
		//静态共有方法
		display: function() {}
	};
	
	return _book;
	
})();
```  

### 继承

  简单讲，每个类都有3个部分，第一部分是构造函数内的，这是供实例化对象复制用的，第二部分是构造函数外的，直接通过点语法（`.`）添加的，这是供类使用的，实例化对象是访问不到的，第三部分是类原型中的，实例化对象可以通过其原型链间接访问到，也是为提供所有实例化对象所公用的。然而在继承中所涉及的不仅仅是一个对象。
  
#### 类式继承

```javascript
// 声明父类
function SuperClass() {
	this.superValue = true;
}
// 为父类添加共有方法
SuperClass.prototype.getSuperValue = function() {
	return this.superValue;
};

// 声明子类
function SubClass() {
	this.subValue = false;
}

// 继承父类，之类prototype属性指向父类实例，形成原型链
SubClass.prototype = new SuperClass();
// 为子类添加共有方法
SubClass.prototype.getSubValue = function() {
	return this.subValue;
}
```

  此种继承方式有2个缺点。其一，由于子类通过其原型`prototype`对父类实例化，继承了父类。所以说父类中的共有属性要是引用类型，就会在子类中被所以实例公用，因此一个子类的实例更改子类原型冲父类构造器中继承来的共有属性就会直接影响到其他子类（比如下面的代码所示）；其二，由于子类实现的继承是通过原型`prototype`对父类实例化实现的，因此在创建父类的时候，是无法向父类传递参数的，因而在实例化父类的时候无法对父类构造函数内地属性经常初始化。

```javascript
function SuperClass() {
	this.books = ['javascript', 'html', 'css'];
}  

function SubClass() {}

SubClass.prototype = new SuperClass();

var instance1 = new SubClass();
var instance2 = new SubClass();
console.log(instance2.books);  //'javascript', 'html', 'css'
instance1.books.push('设计模式');
console.log(instance1.books);  //'javascript', 'html', 'css', '设计模式'
```

`instance1`的一个无意修改伤害了`instance2`的`books`属性，这在编程中很容易埋藏陷阱。


#### 构造函数式继承

针对第一种方式的缺点进行改进，使用构造函数实现继承。

```javascript
function SuperClass(id) {
	//引用类型共有属性
	this.books = ['javascript', 'html', 'css'];
	this.id = id;  //使用参数实例化属性
}
//父类声明原型方法
SuperClass.prototype.showBooks = function() {
	console.log(this.books);
}
//子类
function SubClass(id) {
	// 继承父类
	SuperClass.call(this, id);
}
// 创建实例
var instance1 = new SubClass(10);
var instance2 = new SubClass(11);
instance1.books.push('设计模式');
console.log(instance1.books);  //'javascript', 'html', 'css', '设计模式'
console.log(instance1.id);  //10
console.log(instance2.books);  //'javascript', 'html', 'css'
console.log(instance2.id); //11
instance1.showBooks();  //TypeError
```

#### 组合式继承

  组合式继承是结合了类式继承和构造函数式继承的特性而来，类式继承是通过子类的原型`prototype`对父类实例化实例来实现，构造函数式继承是通过在子类的构造函数作用环境中执行一次父类的构造函数来实现。
  
```javascript
function SuperClass(name)  {
	//值类型的共有属性
	this.name = name;
	this.books = ['html', 'javascript', 'css'];
}
//父类原型的共有方法
SuperClass.prototype.getName = function() {
	console.log(this.name);
};

//子类
function SubClass(name, time) {
	//构造函数式继承
	SuperClass.call(this, name);
	//子类中新增的共有属性
	this.time = time;
}
//类式继承，指向父类实例，形成原型链
SubClass.prototype = new SuperClass();
//子类原型方法
SubClass.prototype.getTime = function() {
	console.log(this.name);
};

//  -----------------------  测试代码
var instance1 = new SubClass('j book', 2015);
instance1.books.push("设计模式");
console.log(instance1.books);  //'html', 'javascript', 'css' 设计模式
instance1.getName();  // j books
instance1.getTime();  // 2015

var instance2 = new SubClass('css book', 2016);
instance2.books.push("sss");
console.log(instance1.books);  //'html', 'javascript', 'css' sss
instance2.getName();  // csss books
instance2.getTime();  // 2016
```

**缺点：**使用构造函数继承时执行了一遍父类的构造函数，然后在实现子类原型的类式继承时有执行了一遍父类的构造函数，因此父类的构造函数执行了两次。


#### 原型式继承

  此方式借助于原型`prototype`可以根据已有的对象创建一个新的对象，同时不必创建新的自定义对象。
  
```javascript
function inheritObject(0) {
  //声明一个过渡的对象
  function F() {}
  // 过渡对象的原型继承父类对象
  F.prototype = o;
  //返回过渡对象的一个实例，该实例的原型继承了父对象
  return new F();
}
```

####  寄生式继承

  此方式是在原型方式的基础上完善而来，其实就是对原型式继承做二次封装，并且在二次封装中对继承的对象进行拓展，这样新创建的对象不仅仅有父类中的属性和方法，而且还添加新的属性和方法。
  
```javascript
//声明基对象
var book = {
  name: 'js book',
  alikeBook: ['css', 'html']
};
function createBook(obj) {
  //通过原型继承方式创建对象
  var o = new inheritObject(obj);
  //拓展属性
  o.getName = function() {
    console.log(name);
  };
  //返回拓展后的新对象
  return o;
}
```

#### 终极继承方式——寄生组合式继承

  寄生式继承依托于原型继承，原型继承又与类式继承相像。
  
```javascript
/**
 * 寄生式继承   继承原型
 * 传递参数 subClass 子类
 * 传递参数 superClass 父类
**/
function inheritPrototype(subClass, superClass) {
  //复制一份父类的原型副本保存在变量中
  var p = inheritObject(superClass.prototype);
  //修正因为重写子类原型导致子类的contructor属性被修改
  p.constructor = subClass;
  //设置子类的原型
  subClass.prototype = p;
}
```
```
