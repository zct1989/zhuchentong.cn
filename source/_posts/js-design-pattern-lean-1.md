---
title: javascript设计模式学习笔记 - 1.javascript继承
tags:
  - javascript
categories:
  - 前端
date: 2017-11-01 11:59:47
---

去年看书写的笔记一直放在gitosc上，突然看见了，也都放到博客上来。

----

javascript继承
===

面型对象语言(OOP)，拥有三大特征，五大原则。
其中三大特征：
* 封装
* 继承
* 多态

五大原则：
* 单一职责原则
* 开放封闭原则
* 替换原则
* 依赖原则
* 接口分离原则

 这里我们主要来看三大特征中的**继承**在javascript中的表现。


 ### 基础继承方式

 一. 类式继承

 类式继承是通过原型链连接父类实例对象完成的继承。

<!--more-->

 ```javascript
 //声明父类
 function SuperClass(){
   this.superValue = true;
 }

 //添加共有方法
  SuperClass.prototype.getSuperValue = function(){
    return this.superValue;
  }

  //声明子类
  function SubClass(){
    this.subValue = false;
  }

  //继承父类
  SubClass.prototype = new SuperClass();

  //添加子类共有方法
  SubClass.prototype.getSubValue = function(){
    return this.subValue;
  }
 ```
正如上面所看的类式继承是通过将子类的prototype指向父类的实例完成继承，这样使用方法时，对象的__proto__属性会指向当前类的prototype也就是父类的实例查找到父类的属性和方法。

当然类式继承存在着他的缺点，就是子类的不同实例指向的是同一父类的实例对象，容易产生的问题是:

  1. 多个子类实例可以操作这个父类实例对象里的属性，造成互相的干扰和影响。
  2. 类式继承是通过原型对父类实例化，所以创建父类时无法向其传递参数，也无法对父类构造函数进行继承和初始化。

 二. 构造函数继承

 直接来看看什么是构造函数继承。

 ```javascript
  function SuperClass(id){
    //引用类型共有属性
    this.books = ["test1","test2","test3"];
    //值类型共有属性
    this.id = id ;
  }

  //父类声明原型方法
  SuperClass.prototype.showBooks = function(){
    console.log(this.books);
  }

  //声明子类
  function SubClass(id){
    SuperClass.call(this,id);
  }

  var instance = new SubClass(1);

 ```
 构造函数继承主要依赖的就是 call 方法，它改变了函数执行的上下文，把原本父类的构造函数在子类的作用域上进行执行，这样就完成了对构造函数的继承和初始化，可以通过向子类初始化传值，完成父类构造函数对应的初始化。

 由于构造函数继承的方式没有对涉及原型prototype,所有子类原型链中没有关联到父类，无法使用父类原型中的方法，不符合代码复用原则。

 ### 技巧继承方式
 三. 组合继承

 既然类式继承实现了原型链的关联，构造函数式继承完成了对父类构造函数的继承，那么我们综合他们来试一下。

 ```javascript
 function SuperClass(name){
   this.name = name;
   this.books = ["test1","test2","test3"];
 }

 SuperClass.prototype.getName = function(){
   console.log(this.name);
 }

 //声明子类
 function SubClass(name,this){
   SuperClass.call(this,name);
   this.time = time;
 }

 SubClass.prototype = new SuperClass();

 SubClass.prototype.getTime = function(){
   console.log(this.time);
 }

 var instance = new SubClass("jobs",2016);
 ```

 组合继承就是在子类构造函数中执行父类构造函数完成构造函数继承，又在原型上实例化父类完成类式继承，综合了类式继承核构造函数继承的优点，并过滤了他们的缺点。

 他的缺点在于我们在完成子类构造函数时调用了父类的构造函数，而我们在实现类式继承时又对父类进行实例化，所以我们调用了两次父类构造函数。

 四. 原型式继承

 先说一下原型式继承的核心观念：借助原型prototype可以根据已有的对象创建一个新的对象，同时不必创建新的自定义对象类型。

 ```javascript
 //原型式继承
 function inheritObject(o){
   //声明一个过度函数对象
   function F(){};
   //过渡对象的原型继承父对象
   F.prototype = o;
   //返回过渡对象的一个实例，该实例的原型继承了父对象
   return new F();
 }
 ```
原型式继承只不过是对类式继承的一个封装，类式继承的问题他同样存在，但是过渡函数的构造函数中没有内容，这样开销很小。
Object.create()方法就是对原型式继承的一种实现。

五. 寄生式继承

直接来看实现

```javascript
function createBook(obj){
  //通过原型继承方式创建新对象
  var o = new inheritObject(obj);
  //扩展新对象
  o.getName = function(){
    console.log(name);
  }

  return o;
}

```

 好吧，可以看到寄生式继承是对原型继承的二次封装，并且在其中对继承的对象进行了扩展。

 六. 寄生组合式继承

 之前类类式继承中出现的问题是子类不是父类的实例，而子类的原型是父类的实例，现在我们使用寄生组合式继承来解决这个问题，寄生组合继承是由寄生继承和构造函数继承组合起来的继承方式。

 ```javascript
  //寄生式继承
         function inheritPrototype(subClass, superClass) {
  				//复制一份父类的原型副本保存在变量中
  				var p = Object.create(superClass.prototype);
  				//修正因为重写子类原型导致子类的constructor属性被修改
  				p.constructor = subClass;
  				//设置子类的原型
  				subClass.prototype = p;
  			}
  			//定义父类
  			function SuperClass(name) {
  				this.name = name;
  				this.books = ["test1", "test2", "test3"];
  			}
  			SuperClass.prototype.getName = function() {
  				console.log(this.name);
  			}
  			//定义子类
  			function SubClass(name, time) {
  				SuperClass.call(this, name);
  				this.time = time;
  			}
  			//寄生式继承父类原型
  			inheritPrototype(SubClass, SuperClass);
  			//子类原型增强方法
  			SubClass.prototype.getTime = function() {
  				console.log(this.time);
  			}
  			var instance1 = new SubClass("test1",2015);  
  			var instance2 = new SubClass("test2",2016);
  			instance1.getName();
  			instance1.getTime();

  			instance2.getName();
  			instance2.getTime();

  			console.log(instance1 instanceof SuperClass);
 ```
