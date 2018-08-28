---
title: JavaScript专题之解读 v8 排序源码
---

[JavaScript专题之解读 v8 排序源码](https://github.com/mqyqingfeng/Blog/issues/52)

>v8 是 Chrome 的 JavaScript 引擎，其中关于数组的排序完全采用了 JavaScript 实现。
>排序采用的算法跟数组的长度有关，当数组长度小于等于 10 时，采用插入排序，大于 10 的时候，采用快速排序。(当然了，这种说法并不严谨)。
>我们先来看看插入排序和快速排序。

## 一、插入排序
>将第一个元素视为有序序列，遍历数组，将之后的元素依次插入这个构建的有序序列中。

![](./images/insertion_2.gif)
``````javascript?linenums
function insertionSort(arr) {
    for (var i = 1; i < arr.length; i++) {
        var element = arr[i];
        for (var j = i - 1; j >= 0; j--) {
            var tmp = arr[j];![enter description here](./images/insertion_1.gif)
            var order = tmp - element;
            if (order > 0) {
                arr[j + 1] = tmp;
            } else {
                break;
            }
        }
        arr[j + 1] = element;
    }
    return arr;
}

var arr = [6, 5, 4, 3, 2, 1];
console.log(insertionSort(arr));
```