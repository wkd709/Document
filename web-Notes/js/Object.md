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
普通对象中的__proto__是什么呢？ Object的本质函数对象，是通过new Function()创建，所以Object.__ proto __ 指向Function.prototype。同理，Function也是函数对象，因此Function.__ proto__ 同样指向Function.prototype。 Object.prototype对象也有__proto__属性，但它比较特殊，为null。这个由__proto__串起来的直到Object.prototype.__ proto__为null的链就是原型链。

```js?linenums
console.log(Object.__proto__ === Function.prototype);//true
console.log(Function.__proto__===Function.prototype);//true
console.log(Object.prototype.__proto__);//null
```