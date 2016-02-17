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
  //
}

  
