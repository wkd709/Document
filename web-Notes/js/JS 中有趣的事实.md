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
