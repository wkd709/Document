---
title: Object
---
该Object构造函数会为给定值的对象包装程序。如果值为null或undefined，则它将创建并返回一个空对象，否则，它将返回与给定值对应的Type对象。如果值已经是一个对象，它将返回该值。

## 1. Object.length
值为1




## 2. Object.prototype (原型对象 prototype)
Javascript中所有的对象都是Object的实例，并继承Object.prototype的属性和方法，有些属性是隐藏的。


该Object.prototype属性表示Object原型对象

注意：
  * 在函数对象中存在原型对象prototype，在普通对象里没有prototype，但存在 __ proto __  ;
 * 或者说 使用function定义的对象与使用new操作符生程的对象之间有一个重要的区别，这个区别在function定义的对象有一个prototype属性，使用new生成的对象没有prototype属性，存在 **__ proto __**；

```js?linenums
var o =new Object();    
console.log(o.__proto__);
console.log(o.prototype);//undefined

var fn = function(){} 
console.log(fn.prototype);//Object {constructor: function}
var f1 = new fn();
console.log(f1.__proto__);
console.log(f1.__proto__===fn.prototype);//true
```

JavaScript中的几乎所有对象都是**Object**; 典型对象继承属性（包括方法）**Object.prototype**，尽管这些属性可能被遮蔽（也称为被覆盖）。然而，Object可能是故意创建的，这不是真的（例如，通过**Object.create(null)）**，或者可能被改变以使其不再是真的（例如，有**Object.setPrototypeOf**）。

**所有Object对象**都可以通过原型链看到对原型对象的更改，除非沿着原型链进一步覆盖受这些更改影响的属性和方法。这为覆盖或扩展对象行为提供了一种非常强大但有潜在危险的机制。

## 3、原型链

那么__proto__是什么？每个对象都会在其内部初始化一个属性，就是__proto__。
普通对象中的__proto__是什么呢？ **Object的本质函数对象**，是通过new Function()创建，所以Object.__ proto __ 指向Function.prototype。同理，Function也是函数对象，因此Function.__ proto__ 同样指向Function.prototype。 Object.prototype对象也有__proto__属性，但它比较特殊，为null。这个由__proto__串起来的直到Object.prototype.__ proto__为null的链就是原型链。

```js?linenums
console.log(Object.__proto__ === Function.prototype);//true
console.log(Function.__proto__===Function.prototype);//true
console.log(Object.prototype.__proto__);//null
```

当我们访问一个对象的属性 时，如果这个对象内部不存在这个属性，那么他就会去__proto__里找这个属性，这个__proto__又会有自己的__proto__，于是就这样 一直找下去，也就是我们平时所说的原型链的概念。参考下面的例子：

```js?linenums
var Fn = function(){};
Fn.prototype.Hello = function(){
        console.log("Hello World");
    }
var f1 = new Fn();
f1.Hello();//Hello World
```

首先var f1=new fn(),f1是Fn的实例，可以得出f1.__ proto__=Fn.prototype。当我们调用f1.hello()时，首先f1中没有Hello这个属性，于是，它会到他的__proto__中去找，也就是Fn.prototype，而我们在上面定义了 Fn.prototype.Hello=function(){}; 于是，就找到了对应的方法。


从一个更复杂的例子中看原型链的原理：

```js?linenums
var Person = function() {};
Person.prototype.Name = function() {
    console.log("person name");
}
Person.prototype.Sex = "male or female";

var Younger = function() {};
Younger.prototype = new Person();
Younger.prototype.Age = function() {
    console.log("14-28")
};
Younger.prototype.Sex = "female";

var Ann = new Younger();
Ann.Name(); //person name
console.log(Ann.Age()); //14-28
console.log(Ann.Sex); //female
```
对上述代码，我们可以进行如下分析：
var Younger = function() {}===>：Younger.__ proto__=Person.prototype,
Younger.prototype = new Person()===>:Younger.prototype.__ proto__ = Person.prototype,
var Ann = new Younger()===>Ann.__ proto__=Younger.prototype,
综上可得：
Ann.__ proto__.__ proto__ = Person.prototype

Ann本身没有Name()方法，于是从Ann.__ proto__(Younger.prototype)中找,仍没有找到于是在向上一层Ann.__ proto__.__ proto__(Person.prototype)中寻找，最终在Person.prototype中找到对应的方法并调用。
同理，Ann本身并没有Age()方法，但在Ann.__ proto__(Younger.prototype)存在。
对于Ann.Sex,在Ann.__ proto__(Younger.prototype)中已经能够找到，便不再向上寻找，因此输出是female。