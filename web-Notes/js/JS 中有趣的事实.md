---
title: JS 中有趣的事实
---

## 一、NaN 是一个 number 类型

>NaN是一个 number 类型。 而且，NaN 不等于它自己。 实际上NaN不等于任何东西，验证一个变量是否是 NaN 可以使用 isNaN() 方法来判断。

```js
> typeof(NaN)
"number"

> NaN === NaN
false
```

## 二、null 是一个对象

>null是一个对象。 听起来奇怪！ 对？ 但这是事实。

```js?linenums
> typeof(null)
"object"

//在这种情况下，null表示没有值。因此，null不应该是Object的实例。

> null instanceof Object
false    
```

## 三、undefined 可以被定义

>undefined不是 JS 中的保留关键字， 你可以为其指定值也不会报错，如果声明一个变量没有赋值，默认为 undefined

```js?linenums
> var some_var;
undefined
> some_var == undefined
true
> undefined = 'i am undefined'   
```

## 四、0.1 + 0.2 不等于 to 0.3

>在JavaScript中，0.1 +0.2 == 0.3返回false。 事实是，javascript 将浮点数存储为二进制。

```js?linenums
> 0.1 + 0.2
0.30000000000000004
> 0.1 + 0.2 == 0.3
false   
```

## 五、Math.max() 比 Math.min() 小

>Math.max() > Math.min()返回false的事实看起来是错误的，但实际上它是正确的。
>如果没有参数传给min()或max()，那么它将返回以下值。

```js?linenums
> Math.max()
-Infinity
> Math.min()
Infinity
```

## 六、018 - 045 = -19

> 在JavaScript中，前缀0会把任何数字转换成八进制。但是，八进制中不使用8，任何包含8的数字都将被无声地转换为常规的十进制数字。

```js?linenums
> 018 - 045
-19  
```

**因此，**018-019实际上等于十进制表达式18-37，因为045是八进制，但018是十进制。

## 七、函数可以自执行

>只需创建一个函数，并在调用其他函数时立即调用它，并使用 () 语法

```js?linenums
> (function()  { console.log('I am self executing');  })();
I am self executing    
```

## 八、括号的位置问题

>`return` 语句后面没有东西的时候它什么都不返回。 实际上，JS 后面 `return` 添加一个 `;`。
> 如果要return多个值，就得用 { } ；

```js?linenums
> function foo() {
   return
   {
      foo: 'bar'
   }
}
> foo(); 
undefined

> function foo() {
   return {
      foo: 'bar'
   }
}
> foo(); 
{foo: "bar"}
```

## 九、没有整数数据类型

>在 JS 中，没有int（整数）数据类型。 所有数字均为 Number 类型。 实际上它将int数的浮点值存储在内存上。

## 十、sort() 函数自动类型转换

>sort() 函数自动将值转换为字符串，这就会导致奇怪的事情发生。

```js?linenums
> [1,5,20,10].sort()
(4) [1, 10, 20, 5]

//但是，它可以通过比较来解决:

> [1,5,20,10].sort(function(a, b){return a - b});
(4) [1, 5, 10, 20]
```

