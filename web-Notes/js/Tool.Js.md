---
title: Tool.Js
---

## 一、追加当前目录下的css到页面上

例如：

```js?linenums
!function(){
  var cropperObj = {
    version: '1.4.1',
    getPath: function() {
      var e = document.currentScript ? document.currentScript.src: function() {
        for (var e, t = document.scripts,i = t.length - 1,n = i; n > 0; n--) {
          if ("interactive" === t[n].readyState) {
            e = t[n].src;
            break;
          }
          return e || t[i].src;
        }
      }();
      return e.substring(0, e.lastIndexOf("/") + 1);
    }
  };
  var path =  cropperObj.getPath();
  var link = document.createElement("link");
  link.rel = "stylesheet";
  link.href = path + 'cropper.css?v='+cropperObj.version;
  var head = document.getElementsByTagName("head")[0];
  head.appendChild(link);
}();
```

## 二、时间格式转换

```js?linenums
/**
 * 时间转换
 * fmt  例如：yyyy-MM-dd hh:mm:ss
 **/
exports.getDate = function(val, fmt) {
    var date = '';
    if (!val) {
        date = new Date();
    } else if (val) {
        date = new Date(val);
    }
    var o = {
        "M+": date.getMonth() + 1, //月份
        "d+": date.getDate(), //日
        "h+": date.getHours(), //小时
        "m+": date.getMinutes(), //分
        "s+": date.getSeconds(), //秒
        "q+": Math.floor((date.getMonth() + 3) / 3), //季度
        "S": date.getMilliseconds() //毫秒
    };
    if (/(y+)/.test(fmt))
        fmt = fmt.replace(RegExp.$1, (date.getFullYear() + "").substr(4 - RegExp.$1.length));
    for (var k in o)
        if (new RegExp("(" + k + ")").test(fmt))
            fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
    return fmt;
};
```