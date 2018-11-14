---
title: ToolJs
---

## 一、追加当前目录下的css到页面上

例如：

```js
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
        }
        return e || t[i].src;
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

```js
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

## 三、根据长度截取先使用字符串，超长部分追加…

```js
/**参数说明：
 * 根据长度截取先使用字符串，超长部分追加…
 * str 对象字符串
 * len 目标字节长度
 * 返回值： 处理结果字符串
 */
exports.cutString = function(str, len) {
    if (!str) {
       return '';
    }
    if (len) {//截取长度时，去掉标签字符
        //去除标签字符
        str = str.replace(/(<|&lt;)\/?.+?(>|&gt;)/g,"")
            .replace(/(&nbsp;|nbsp;|&amp;|amp;|&quot;|quot;)/ig,"")
            .replace(/ /g,"");
    } else {//完整html时
        //字符转为html标签
        var arrEntities={'lt':'<','gt':'>','nbsp':' ','amp':'&','quot':'"'};
        str =  str.replace(/&(lt|gt|nbsp|amp|quot);/ig,function(all,t){return arrEntities[t];});
    }

    if (str.length * 2 <= len) {
        return str;
    }

    var strlen = 0;
    var s = "";
    for (var i = 0; i < str.length; i++) {
        s = s + str.charAt(i);
        if (str.charCodeAt(i) > 128) {
            strlen = strlen + 2;
            if (strlen >= len) {
                return s.substring(0, s.length - 1) + "...";
            }
        } else {
            strlen = strlen + 1;
            if (strlen >= len) {
                return s.substring(0, s.length - 2) + "...";
            }
        }
    }
    return s;
};
```

## 四、保留2位小数

```js
/**
 * 保留两位小数的方法
 * @param {*} num 传入的数字
 **/
exports.toDecimal2 = function(num) {
    var f = parseFloat(num);
    if (isNaN(f)) {
        return false;
    }
    var f = (Math.round(num * 100) / 100).toFixed(2);
    var s = f.toString();
    var rs = s.indexOf('.');
    if (rs < 0) {
        rs = s.length;
        s += '.';
    }
    while (s.length <= rs + 2) {
        s += '0';
    }
    return s;
}
```

## 五、获取一周的时间

```js
// 获取一周的日期
exports.getWeek = function(datevalue) {
    var week = [];
    var weekName = [
        {num:0,name: '星期一'},
        {num:1,name: '星期二'},
        {num:2,name: '星期三'},
        {num:3,name: '星期四'},
        {num:4,name: '星期五'},
        {num:5,name: '星期六'},
        {num:6,name: '星期天'},
    ];
    var index = 0;
    for (var i = -3 ; i < 4; i++) {
        if(datevalue) {
            var stamp = new Date(datevalue);
        } else {
            var stamp = new Date();
        }
        stamp.setDate(stamp.getDate() + i);
        
        week[index] = {
            date: stamp.getFullYear() +'-',
        };
        
        if(stamp.getMonth() < 9 && stamp.getMonth() >= 0) {
            week[index].date = week[index].date+'0'+(stamp.getMonth()+1)+'-';
        } else {
            week[index].date = week[index].date+(stamp.getMonth()+1)+'-';
        }
        
        if(stamp.getDate() < 10 && stamp.getDate() > 0) {
            week[index].date = week[index].date+'0'+stamp.getDate();
        } else {
            week[index].date = week[index].date+stamp.getDate();
        }
        
        weekName.forEach(function (item) {
            if(item.num == stamp.getDay()) {
                week[index].weekDate = item.name;
            }
        })
        index++;
    }
    return week;
}

```

六、判断类型

Object.prototype.toString.call();

```js?linenums
var a = 1;
Object.prototype.toString.call(a);
//"[object Number]"
var a = function() {};
Object.prototype.toString.call(a);
//"[object Function]"
var a = 'dd';
Object.prototype.toString.call(a);
//"[object String]"
var a = null;
Object.prototype.toString.call(a);
//"[object Null]"
var a = undefined;
Object.prototype.toString.call(a);
//"[object Undefined]"
var a = [1,1];
Object.prototype.toString.call(a);
//"[object Array]"
var a = {};
Object.prototype.toString.call(a);
//"[object Object]"
//var a = /$s/;
Object.prototype.toString.call(a);
//"[object RegExp]"
var a = new Date();
Object.prototype.toString.call(a);
//"[object Date]"
var a = true;
Object.prototype.toString.call(a);
//"[object Boolean]"


//判断json
var a = '{"name":"dd"}';
if (typeof str == 'string') {
   var obj=JSON.parse(a);
   if (Object.prototype.toString.call(obj).split(' ') == 'Object') {
        return true;//json
  }
}
```

```js?linenums
function getClass (a) {
  const str = Object.prototype.toString.call(a)
  return /^\[object (.*)\]$/.exec(str)[1]
}
```

## 六、是否安装flash

```js?linenums
function flashChecker() {
    var hasFlash = 0;　　　　 //是否安装了flash
    var flashVersion = 0;　　 //flash版本
    if(document.all) {
        var swf = new ActiveXObject('ShockwaveFlash.ShockwaveFlash');
        if(swf) {
            hasFlash = 1;
            VSwf = swf.GetVariable("$version");
            flashVersion = parseInt(VSwf.split(" ")[1].split(",")[0]);
        }
    } else {
        if(navigator.plugins && navigator.plugins.length > 0) {
            var swf = navigator.plugins["Shockwave Flash"];
            if(swf) {
                hasFlash = 1;
                var words = swf.description.split(" ");
                for(var i = 0; i < words.length; ++i) {
                    if(isNaN(parseInt(words[i]))) continue;
                    flashVersion = parseInt(words[i]);
                }
            }
        }
    }
    return { f: hasFlash, v: flashVersion };
}

var fls = flashChecker();
var s = "";
if(!fls.f) {
    if (confirm("您的浏览器未安装Flash插件，现在安装？")) {
        window.location.href = "http://get.adobe.com/cn/flashplayer/";
    }
}
```

## 七、生成随机数的方法汇总

### 7.1、随机浮点数的生成

####  7.1.1）、生成 [ 0, 1 ) 范围内的随机数（大于等于0，小于1）

使用 random() 方法可以返回一个介于 0 ~ 1 之间的伪随机数（包括 0，不包括 1）,  **Math.random()**

####  7.1.2）、生成 [ n, m ) 范围内的随机数（大于等于n，小于m）,**Math.random()*(m-n)+n**


```js?linenums
//下面生成 [10,15) 范围内的随机浮点数。
var random1 = Math.random()*(15-10)+10;
var random2 = Math.random()*(15-10)+10;
var random3 = Math.random()*(15-10)+10;
console.log(random1);
console.log(random2);
console.log(random3);
```

####  7.1.3）、生成 [n,m]、(n,m)、(n,m] 范围内的随机数

```js?linenums
//取得[n,m]范围随机数
function fullClose(n,m) {
   var result = Math.random()*(m+1-n)+n;
   while(result>m) {
       result = Math.random()*(m+1-n)+n;
   }
   return result;
}

 
//取得(n,m)范围随机数
function fullOpen(n,m) {
   var result = Math.random()*(m-n)+n;
   while(result == n) {
       result = Math.random()*(m-n)+n;
   }
   return result;
}
 
//取得(n,m]范围随机数
function leftOpen(n,m) {
   var result = Math.random()*(m-n+1)+n-1;
   while(result<n) {
       result = Math.random()*(m-n+1)+n-1;
   }
   return result;
}
```

### 7.2、随机整数的生成

#### 7.2.1）、随机生成 0、1 这两个整数

![](./images/1542167561823.png)