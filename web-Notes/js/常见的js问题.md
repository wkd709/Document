---
title: 常见的js问题 
---

## 一、你所忽略的js隐式转换

### 1、js数据类型
>js中有7种数据类型，可以分为两类：原始类型、对象类型。
>基础类型（原始值）：
>Undefined、Null、String、Number、Boolean、Symbol（es6）
>复杂类型（对象值）：
>Object

### 2、三种隐式转换类型

>涉及隐式转换最多的两个运算符 + 和 ==。

隐式转换中主要涉及到三种转换：

* 1、将值转为原始值，ToPrimitive()。
* 2、将值转为数字，ToNumber()。
* 3、将值转为字符串，ToString()。

#### 2.1、通过ToPrimitive将值转换为原始值。
js引擎内部的抽象操作ToPrimitive有这这样的签名：
```js
ToPrimitive(input, PreferredType?)
```

input 是要转换的值，PreferredType是可选参数，可以是Number或String类型。他只是一个转换标志，转化后的结果并不一定是这个参数所值的类型，但转换结果一定是一个原始值（或报错）。

##### 2.1.1如果PreferredType被标记为Number，则会进行下面的操作流程来转换输入的值。
```tex?linenums
1、如果输入的值已经是一个原始值，则直接返回它
2、否则，如果输入的值是一个对象，则调用该对象的valueOf()方法，
   如果valueOf()方法的返回值是一个原始值，则返回这个原始值。
3、否则，调用这个对象的toString()方法，如果toString()方法返回的是一个原始值，则返回这个原始值。
4、否则，抛出TypeError异常。
```
##### 2.1.2、如果PreferredType被标记为String，则会进行下面的操作流程来转换输入的值。

```tex?linenums
1、如果输入的值已经是一个原始值，则直接返回它
2、否则，调用这个对象的toString()方法，如果toString()方法返回的是一个原始值，则返回这个原始值。
3、否则，如果输入的值是一个对象，则调用该对象的valueOf()方法，
   如果valueOf()方法的返回值是一个原始值，则返回这个原始值。
4、否则，抛出TypeError异常。
```

既然PreferredType是可选参数，那么如果没有这个参数时，怎么转换呢？PreferredType的值会按照这样的规则来自动设置：

```tex?linenums
1、该对象为Date类型，则PreferredType被设置为String
2、否则，PreferredType被设置为Number
```

##### 2.1.3、valueOf方法和toString方法解析
对于js的常见内置对象：Date, Array, Math, Number, Boolean, String, Array, RegExp, Function。

1、Number、Boolean、String这三种构造函数生成的基础值的对象形式，通过valueOf转换后会变成相应的原始值。如：

```js?linenums
var num = new Number('123');
num.valueOf(); // 123

var str = new String('12df');
str.valueOf(); // '12df'

var bool = new Boolean('fd');
bool.valueOf(); // true
```
2、Date这种特殊的对象，其原型Date.prototype上内置的valueOf函数将日期转换为日期的毫秒的形式的数值。

```js?linenums
var a = new Date();
a.valueOf(); // 1515143895500
```

3、除此之外返回的都为this，即对象本身：

```js?linenums
var a = new Array();
a.valueOf() === a; // true

var b = new Object({});
b.valueOf() === b; // true
```
再来看看toString函数，其转换结果是什么？对于js的常见内置对象：Date, Array, Math, Number, Boolean, String, Array, RegExp, Function。


1、Number、Boolean、String、Array、Date、RegExp、Function这几种构造函数生成的对象，通过toString转换后会变成相应的字符串的形式，因为这些构造函数上封装了自己的toString方法。如：

```js?linenums
Number.prototype.hasOwnProperty('toString'); // true
Boolean.prototype.hasOwnProperty('toString'); // true
String.prototype.hasOwnProperty('toString'); // true
Array.prototype.hasOwnProperty('toString'); // true
Date.prototype.hasOwnProperty('toString'); // true
RegExp.prototype.hasOwnProperty('toString'); // true
Function.prototype.hasOwnProperty('toString'); // true

var num = new Number('123sd');
num.toString(); // 'NaN'

var str = new String('12df');
str.toString(); // '12df'

var bool = new Boolean('fd');
bool.toString(); // 'true'

var arr = new Array(1,2);
arr.toString(); // '1,2'

var d = new Date();
d.toString(); // "Wed Oct 11 2017 08:00:00 GMT+0800 (中国标准时间)"

var func = function () {}
func.toString(); // "function () {}"
```

除这些对象及其实例化对象之外，其他对象返回的都是该对象的类型都是继承的Object.prototype.toString方法。

```js?linenums
var obj = new Object({});
obj.toString(); // "[object Object]"

Math.toString(); // "[object Math]"
```

valueOf函数会将Number、String、Boolean基础类型的对象类型值转换成 基础类型，Date类型转换为毫秒数，其它的返回对象本身，而toString方法会将所有对象转换为字符串。显然对于大部分对象转换，valueOf转换更合理些，因为并没有规定转换类型，应该尽可能保持原有值，而不应该想toString方法一样，一股脑将其转换为字符串。

所以对于没有指定PreferredType类型时，先进行valueOf方法转换更好，故将PreferredType设置为Number类型。

而对于Date类型，其进行valueOf转换为毫秒数的number类型。在进行隐式转换时，没有指定将其转换为number类型时，将其转换为那么大的number类型的值显然没有多大意义。（不管是在+运算符还是==运算符）还不如转换为字符串格式的日期，所以默认Date类型会优先进行toString转换。故有以上的规则：

PreferredType没有设置时，Date类型的对象，PreferredType默认设置为String，其他类型对象PreferredType默认设置为Number。

#### 2.2、通过ToNumber将值转换为数字

![](./images/1541136402221.png)

