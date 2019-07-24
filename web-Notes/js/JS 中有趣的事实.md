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