---
title: javascript设计模式学习笔记 - 2.三种工厂模式
tags:
  - javascript
categories:
  - 前端
date: 2017-11-01 12:01:06
---

简单工厂模式
---

简单工厂模式是通过一个工厂对象统一的去创建对象，避免分散的通过new进行对象的创建，这样factory可以统一的对对象的创建进行控制,消除了对象之间的耦合.

例：
```javascript
    /**
     * type1类型
     * @return {[type]} [description]
     */
    function type1() {};
    type1.prototype.do = function() {
      console.log("this is type1");
    };
    /**
     * type2类型
     * @return {[type]} [description]
     */
    function type2() {};
    type2.prototype.do = function() {
      console.log("this is type2");
    };

    /**
     * 对象工厂
     */
    function Factory() {
      this.sender = null;
    }
    /**
     * 创建对象方法
     * @param  {[type]} type
     * @return {[type]}
     */
    Factory.prototype.create = function(type) {
      switch (type) {
        case "type1":
          this.sender = new type1();
          break;
        case "type2":
          this.sender = new type2();
          break;
        default:
          break;
      }
      return this.sender
    };

    //创建工厂
    var factory = new Factory();
    factory.create("type1").do();
    factory.create("type2").do();
```

<!--more-->

工厂模式分为**简单工厂**和**抽象工厂**两种下来来看一看抽象工厂。

抽象工厂模式
---

在之前的简单工厂模式里面，我们出创建对象已经可以通过工厂进行创建，但是如果我们需要扩展程序那么就需要修改工厂类，这样违背了闭包原则－*对扩展开发，对修改关闭*。

下来来看看抽象工厂，抽象工厂模式定义：**为创建一组相关或相互依赖的对象提供一个接口，而且无需指定他们的具体类。**

抽象工厂的目的是通过对类的工厂进行抽象，完成其业务对于产品簇的创建。

它需要完成
产品抽象类
工厂抽象类
产品子类

产品子类通过抽象工厂对产品抽象类进行继承。

抽象工厂中完成的是将子类的原型指向抽象类实例。

和之前的工厂不同的是工厂方法模式和简单工厂模式是创建的子类的实力，而抽象工厂是创建了子类的类簇。

例子:
```javascript
    var AbstractFacroty = function() {};
    AbstractFacroty.prototype = {
      createProductA: function() {},
      createProductB: function() {}
    };

    var AbstractProductA = function() {};
    AbstractProductA.prototype = {
      do:function(){},
      go:function(){}
    }
    var AbstractProductB = function() {};
    AbstractProductB.prototype = {
      do:function(){},
      go:function(){}
    }

    var ProductA1 = function() {};
    ProductA1.prototype = Object.create(AbstractProductA.prototype);
    ProductA1.prototype.go = function(){
      console.log("this is a1 go");
    }
    ProductA1.prototype.do = function(){
      console.log("this is a1 do");
    }
    var ProductA2 = function() {};
    ProductA2.prototype = Object.create(AbstractProductA.prototype);
    ProductA2.prototype.go = function(){
      console.log("this is a2 go");
    }
    ProductA2.prototype.do = function(){
      console.log("this is a2 do");
    }
    var ProductB1 = function() {};
    ProductB1.prototype = Object.create(AbstractProductB.prototype);
    ProductB1.prototype.go = function(){
      console.log("this is b1 go");
    }
    ProductB1.prototype.do = function(){
      console.log("this is b1 do");
    }
    var ProductB2 = function() {};
    ProductB2.prototype = Object.create(AbstractProductB.prototype);
    ProductB2.prototype.go = function(){
      console.log("this is b2 go");
    }
    ProductB2.prototype.do = function(){
      console.log("this is b2 do");
    }
    var ConcretFactory1 = function() {};
    ConcretFactory1.prototype = Object.create(AbstractFacroty.prototype);
    ConcretFactory1.prototype.createProductA = function() {
      return new ProductA1();
    }
    ConcretFactory1.prototype.createProductB = function() {
      return new ProductB1();
    }

    var ConcretFactory2 = function() {};
    ConcretFactory2.prototype = Object.create(AbstractFacroty.prototype);
    ConcretFactory2.prototype.createProductA = function() {
      return new ProductA2();
    }
    ConcretFactory2.prototype.createProductB = function() {
      return new ProductB2();
    }

    var af = new ConcretFactory1();
    var product1 = af.createProductA();
    var product2 = af.createProductB();
    product1.go();
    product1.do();
    product2.go();
    product2.do();
```

抽象工厂实质是提出工厂类的借口通过添加工厂类进行扩展，我认为在javascript中比较难的地方在于对__proto__和prototype的理解上。

工厂方法模式
---

工厂方法模式:定义一个用于创建对象的接口,让子类决定实例化哪个类,工厂方法使一个类的实例化延迟到其子类。

将对象的基类放到对工厂的原型中去。

例子:
```javascript
    var Factory = function(type,content){
        return this[type](content);
    }

    Factory.prototype = {
      item1 :function(content){
          this.name = content+":item1";
          this.prototype.go = function(){
            console.log(this.name);
          }
      },
      item2 :function(content){
          this.name = content+":item2";
          this.prototype.go = function(){
            console.log(this.name);
          }
      }
    }

    var item = new Factory("item1","test");
    item.go();
```

工厂方法把对象基类的选择交给了[type]这样可以不再书写switch，避免了添加基类时需要不断修改工厂内部逻辑。

简单工厂，工厂方法，抽象工厂三种设计模式先到这里。

结束之前我们来看看 **安全模式类**
如果实例化类时忘记写new关键字，会导致程序异常，安全模式即是在保证缺少new关键字的情况下，实例化依然可以返回正确的结果。

```javascript
  var Demo = function(){
    //安全模式
    if(!(this instanceof Demo)){
      return new Demo();
    }
  };
  Demo.prototype = {
    show : function(){
      console.log("成功获取");
    }
  }

  //正确调用
  var d = new Demo();
  d.show();
  //异常调用
  var d = Demo();
  d.show();
```
