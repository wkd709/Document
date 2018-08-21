---
title: Object
---
该Object构造函数会为给定值的对象包装程序。如果值为null或undefined，则它将创建并返回一个空对象，否则，它将返回与给定值对应的Type对象。如果值已经是一个对象，它将返回该值。

## 1. Object.length
值为1

## 2. Object.prototype

该Object.prototype属性表示Object原型对象

JavaScript中的几乎所有对象都是**Object**; 典型对象继承属性（包括方法）**Object.prototype**，尽管这些属性可能被遮蔽（也称为被覆盖）。然而，Object可能是故意创建的，这不是真的（例如，通过**Object.create(null)）**，或者可能被改变以使其不再是真的（例如，有**Object.setPrototypeOf**）。