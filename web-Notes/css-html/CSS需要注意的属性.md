---
title: CSS需要注意的属性
---

## 一、vertical-align

>用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式。
>取值：baseline | sub | super | text-top | text-bottom | middle | top | bottom | < percentage > | < length >

### 1、取值 (对于行内(inline)元素)

大部分取值是相对于父元素来说的：

* **baseline**

   默认值。 元素基线与父元素的基线对齐。
   
   图像将与文本基线处的文本对齐。请注意，字母上的下降点低于基线。图像不与下降点的最低点对齐，这不是基线。
   
   ![baseline](./images/baseline2.png)
   
* **middle**

  元素的中心与父元素的基线加上小写x一半的高度值对齐。
  
  ![middle](./images/vert-align.png)
  
* **text-bottom**

  与类型的基线不同，是文本的底部，下降到的地方。图像也可以与此深度对齐：
  
  ![text-bottom](./images/text-botto.png)
 
* **text-top**

  与text-bottom相反，是text-top，是当前font-size的最高点。您也可以与此对齐。请注意，当前字体Georgia可能有一些比这里所示的更高的上升，因此差距很小。
  
  ![text-top](./images/text-top.png "text-top")
  
* **top**

   元素及其后代的顶端与整行的顶端对齐。
   
* **bottom**

  元素及其后代的底端与整行的底端对齐。
  
* **sub**

    元素基线与父元素的下标基线对齐。
	
	![enter description here](./images/subandsuper.png "subandsuper")
* **super**

   元素基线与父元素的上标基线对齐。
   
   ![enter description here](./images/subandsuper.png "subandsuper")
   
### 2、取值 (对于table-cell元素)

* **baseline**

   与同行单元格的基线对齐。
   
* **top**

   单元格的内边距的上边缘与行的顶端对齐。
   
* **middle**

   单元格垂直居中。
   
* **bottom**
  
  单元格的内边距的下边缘与行的底端对齐。

* 可以取负值。

### 3、注意

 ### img在严格模式下（DOCTYPE），下面会有几像素的空白。
 
 图片文字等inline元素默认是和父级元素的baseline对齐的，即：vertical-align 的默认值是 baseline；而baseline又和父级底边bottom有一定距离；
 
 img出现的空白就是baseline和bottom之间的这段距离；即使只有图片没有文字，只要是 inline 的图片这段空白都会存在。
 
 img 标签 是inline 元素， inline元素默认是baseline对齐的。 当baseline对齐的时候，baseline和bottom之间有段距离，就是出现的空白；
 
 
 ![](./images/img.png "img")
 
 
 
*  top 和 bottom 之间的值即为 line-height。假如把 line-height 设置为0，那么 baseline 与 bottom 之间的距离也变为0，那道空白也就不见了。
 
 ![](./images/line-height=0.png "line-height=0")
 
 *  如果没有设置 line-height，line-height 的默认值是基于 font-size 的，视渲染引擎有所不同，但一般是乘以一个系数（比如1.2）。因此，在没有设置 line-height 的情况下把 font-size 设为0也可以达到同样的效果。当然，这样做的后果就是不能图文混排了。
 
 ![](./images/font-size=0.png "font-size=0")
 
 * img 设置为块级元素
 
 * 设置vertical-align的属性值

## 二、font-size

```html?linenums
<p>
	<span class="a">Ba</span>
	<span class="c">Ba</span>
	<span class="b">Ba</span>
</p>
```

```css
p span{ background: #eee;}
p { font-size: 100px; }
.a { font-family: Helvetica; }
.b { font-family: STLiti; }
.c { font-family: 	KaiTi; }
```
![](./images/font-size.png "font-size")

font-size 相同，font-family 不同，得到的 span 元素的高度也不同。

**原因在于字体本身，这是字体的原理**：

* 一款字体会定义一个[em-square](http://designwithfontforge.com/zh-CN/The_EM_Square.html)，它是用来盛放字符的金属容器。这个em-square 一般被设定为宽高均为 1000 相对单位，不过也可以是 1024、2048 相对单位。

![](./images/v2-4c684524b8d37789295f6db964201c74_hd.jpg)

* 字体度量都是基于这个相对单位设置的，包括 ascender、descender、capital height、x-height 等。注意这里面的值是允许相对于 em-square 出血（bleed outside）的（译者注：大概可以理解为超出 em-square）

![](./images/v2-7f1590fecc2f79484b22f38e2869d83b_hd.jpg)

* 在浏览器中，上面的 1000 相对单位会按照你需要的 font-size 缩放。

## 三、line-height

 CSS 属性用于设置多行元素的空间量，比如文本。对于块级元素，它指定元素行盒（line boxes）的最小高度。对于非替代的inline元素，它用于计算行盒（line box）的高度。
 
 **取值**：
 
   * **normal**
   
     取决于用户端。桌面浏览器（包括Firefox）使用默认值，约为1.2，这取决于元素的 font-family。
  
  * **<数字>**

    该属性的应用值是这个无单位数字乘以该元素的字体大小。计算值与指定值相同。大多数情况下，这是设置line-height的推荐方法，不会在继承时产生不确定的结果。
 
 * **<长度>**

   指定<长度>用于计算 line box 的高度。查看<长度> 获取可能的单位。以em为单位的值可能会产生不确定的结果（见下面的例子）。
      