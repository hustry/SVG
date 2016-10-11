
## SVG 学习笔记

实例整理自[Mozilla](https://developer.mozilla.org/zh-CN/docs/Web/SVG/Tutorial)

## 目录

* [入门](https://github.com/hustry/SVG/tree/master/01-StartUp)

* [填充与边框](https://github.com/hustry/SVG/tree/master/02-FillStroke)

* [渐变](https://github.com/hustry/SVG/tree/master/03-Gradient)

* [图案Patterns](https://github.com/hustry/SVG/tree/master/04-Patterns)

* [文字Texts](https://github.com/hustry/SVG/tree/master/05-Texts)

* [基本形状](https://github.com/hustry/SVG/tree/master/07-BasicShape)

* [路径](https://github.com/hustry/SVG/tree/master/08-path)

* [变形](https://github.com/hustry/SVG/tree/master/09-transform)

* [剪切与遮罩](https://github.com/hustry/SVG/tree/master/10-Clipping_Masking)

### 入门
	
**Basic properties of SVG files**

* SVG可以直接嵌入HTNL5.

* 可以通过object对象引入

```html
<object data="image.svg" type="image/svg+xml" />
```

* 可以使用iframe

```html
<iframe src="image.svg"></iframe>
```
**SVG File Types**

两种格式,常规.svg格式,压缩版svg格式.svgz(兼容性不好).

### Positions

**网格**

对于所有元素，SVG使用的坐标系统或者说网格系统，和Canvas用的差不多（所有计算机绘图都差不多）。这种坐标系统是：以页面的左上角为(0,0)坐标点，坐标以像素为单位，x轴正方向是向右，y轴正方向是向下

```html
<rect x="0" y="0" width="100" height="100" />
```

**viewBox属性**

viewBox属性定义了画布上可以显示的区域：从(0,0)点开始，100宽*100高的区域。这个100*100的区域，会放到200*200的画布上显示。于是就形成了放大两倍的效果.

```html
<svg width="200" height="200" viewBox="0 0 100 100">
```

### 填充与边框

* fill              设置对象内部的颜色
* stroke            设置线条颜色
* fill-opacity      填充色的不透明度
* stroke-opacity    描边的不透明度
* stroke-width      描边的宽度
* stroke-linecap    边框终点的形状[butt,square,round]
* stroke-linejoin   描边线段之间用什么方式[miter,round,bevel]
* stroke-dasharray  将边框定义成虚线[实线长度,虚线长度,...]



```html
  <line x1="40" x2="120" y1="20" y2="20" stroke="black" stroke-width="20" stroke-linecap="butt"/>
  <line x1="40" x2="120" y1="60" y2="60" stroke="black" stroke-width="20" stroke-linecap="square"/>
  <line x1="40" x2="120" y1="100" y2="100" stroke="black" stroke-width="20" stroke-linecap="round"/>
```

**使用CSS**

SVG规范将属性区分成properties和其他attributes，前者是可以用CSS设置的，后者不能。

语法和在html里使用CSS一样，只不过你要把background-color、border改成fill和stroke。注意，不是所有的属性都能用CSS来设置。上色和填充的部分一般是可以用CSS来设置的，比如fill，stroke，stroke-dasharray等，但是不包括下面会提到的渐变和图案等功能。另外，width、height，以及路径的命令等等，都不能用css设置

行内style定义.

```html
<rect x="10" height="180" y="10" width="180" style="stroke: black; fill: red;"/>
```

也可以使用hover这样的伪类来创建翻转之类的效果.

```css
#MyRect:hover {
   stroke: black;
   fill: blue;
 }
```

### 渐变

渐变定义必须定义在defs中.

**线性渐变**

线性渐变内部有几个`stop` 结点，这些结点通过指定位置的`offset`(偏移)属性和`stop-color`(颜色中值)属性来说明在渐变的特定位置上应该是什么颜色；可以直接指定这两个属性值，也可以通过CSS来指定他们的值

```html
<stop offset="100%" stop-color="yellow" stop-opacity="0.5"/>
```

`linearGradient`元素还需要一些其他的属性值，它们指定了渐变的大小和出现范围。渐变的方向可以通过两个点来控制，它们分别是属性x1、x2、y1和y2，这些属性定义了渐变路线走向.

使用url引用defs中定义的渐变作为填充.

```
fill="url(#RadialGradient1)"
```

```html
<linearGradient id="Gradient2" x1="0" x2="0" y1="0" y2="1">
```

**径向渐变**

径向渐变也是通过两个点来定义其边缘位置，两点中的第一个点定义了渐变结束所围绕的圆环，它需要一个中心点，由`cx`和`cy`属性及半径`r`来定义，通过设置这些点我们可以移动渐变范围并改变它的大小.

第二个点被称为焦点，由`fx`和`fy`属性定义。第一个点描述了渐变边缘位置，焦点则描述了渐变的中心

```html
<radialGradient id="Gradient"
    cx="0.5" cy="0.5" r="0.5" fx="0.25" fy="0.25">
<stop offset="0%" stop-color="red"/>
<stop offset="100%" stop-color="blue"/>
</radialGradient>
```

### 图案Patterns

与渐变相同,pattern必须在defs中定义.

```JavaScript
    <pattern id="Pattern" x="0" y="0" width=".25" height=".25">
      <rect x="0" y="0" width="50" height="50" fill="skyblue"/>
      <rect x="0" y="0" width="25" height="25" fill="url(#Gradient2)"/>
      <circle cx="25" cy="25" r="20" fill="url(#Gradient1)" fill-opacity="0.5"/>
    </pattern>
```

### 文字Texts

两种不同的文本,一种是写在图像中的文本,另一种是SVG字体.

**基础**

属性x和属性y性决定了文本在视口中显示的位置。属性text-anchor，可以有这些值：start、middle、end或inherit，允许决定从这一点开始的文本流的方向,和形状元素类似，属性fill可以给文本填充颜色，属性stroke可以给文本描边


```JavaScript
<text x="10" y="10">Hello world!</text>
```

字体属性,下列每个属性可以被设置为一个SVG属性或者成为一个CSS声明：font-family、font-style、font-weight、font-variant、font-stretch、font-size、font-size-adjust、kerning、letter-spacing、word-spacing和text-decoration.


**tspan元素**

该元素用来标记大块文本的子部分，它必须是一个text元素或别的tspan元素的子元素.

```JavaScript
<text>
  <tspan font-weight="bold" fill="red">This is bold and red</tspan>
</text>
```
rotate属性可以将每个字符旋转一个角度

**tref元素**

terf元素允许引用已经定义的文本，高效地把它复制到当前位置.


```html
<text id="example">This is an example text.</text>
<text>
    <tref xlink:href="#example" />
</text>
```

**textPath**

该元素利用它的xlink:href属性取得一个任意路径，把字符对齐到路径，于是字体会环绕路径、顺着路径走.

```html
<path id="my_path" d="M 20,20 C 40,40 80,40 100,20" />
<text>
  <textPath xlink:href="#my_path">This text follows a curve.</textPath>
</text>
```
## Basic Shape

**矩形**

```html
<rect x="60" y="10" rx="10" ry="10" width="30" height="30"/>
```

* x:左上角x位置
* y:矩形左上角y位置
* width:矩形宽度
* height:矩形高度
* rx:圆角x方位半径
* ry:圆角y方位半径

**圆形**

```html
<circle cx="25" cy="75" r="20"/>
```

* r:圆半径
* cx:圆心x位置
* cy：圆心y位置

**椭圆**

```html
<ellipse cx="75" cy="75" rx="20" ry="5" />
```

* rx:椭圆x半径
* ry:椭圆y半径
* cx:椭圆中心x位置
* cy:椭圆中心y位置

**线条**

```html
<line x1="10" x2="50" y1="110" y2="150"/>
```

* x1:起点x位置
* y1:起点y位置
* x2:终点x位置
* y2:终点y位置

**折线**

Polyline是一组连接在一起的直线.因为它可以有很多的点，折线的的所有点位置都放在一个points属性中.

```html
<polyline points="60 110, 65 120, 70 115, 75 130, 80 125, 85 140, 90 135, 95 150, 100 145"/>
```

每个数字用空白、逗号、终止命令符或者换行符分隔开。每个点必须包含2个数字，一个是x坐标，一个是y坐标.

**多边形**

polygon和折线很像，它们都是由连接一组点集的直线构成。不同的是，polygon的路径在最后一个点处自动回到第一个点.

```html
<polygon points="50 160, 55 180, 70 180, 60 190, 65 205, 50 195, 35 205, 40 190, 30 180, 45 180"/>
```

**路径**

path可能是SVG中最常见的形状。你可以用path元素绘制矩形（直角矩形或者圆角矩形）、圆形、椭圆、折线形、多边形，以及一些其他的形状，例如贝塞尔曲线、2次曲线等曲线.

```html
<path d="M 20 230 Q 40 205, 50 230 T 90230"/>
```

```html
<path d="M 20 230 Q 40 205, 50 230 T 90230"/>
```

## Path路径

path元素是SVG基本形状中最强大的一个，它不仅能创建其他基本形状，还能创建更多其他形状.

path元素的形状是通过属性d定义的，属性d的值是一个“命令+参数”的序列.

每一个命令都用一个关键字母来表示，比如，字母“M”表示的是“Move to”命令，当解析器读到这个命令时，它就知道你是打算移动到某个点。跟在命令字母后面的，是你需要移动到的那个点的x和y轴坐标。比如移动到(10,10)这个点的命令，应该写成“M 10 10”。这一段字符结束后，解析器就会去读下一段命令。每一个命令都有两种表示方式，一种是用大写字母，表示采用绝对定位。另一种是用小写字母，表示采用相对定位（例如：从上一个点开始，向上移动10px，向左移动7px）.

属性d采用的是用户坐标系统，所以不需标明单位.

**直线命令**

```
M x y 或者 m dx dy   //移动

L x y 或者 l dx dy   //画线

H x 或者 h dx        //水平线

V y 或者 v dy        //垂直线

```

**曲线Curve**

绘制平滑曲线的命令有三个，其中两个用来绘制贝塞尔曲线，另外一个用来绘制弧形或者说是圆的一部分.

path元素里，只存在两种贝塞尔曲线：三次贝塞尔曲线C，和二次贝塞尔曲线Q


**三次贝塞尔曲线**

三次贝塞尔曲线需要定义一个点和两个控制点，所以用C命令创建三次贝塞尔曲线，需要设置三组坐标参数.

```html
C x1 y1, x2 y2, x y
```

**二次贝塞尔曲线**

二次贝塞尔曲线Q，它比三次贝塞尔曲线简单，只需要一个控制点，用来确定起点和终点的曲线斜率。因此它需要两组参数，控制点和终点坐标.

```html
Q x1 y1, x y
```

**弧形**

弧形可以视为圆形或椭圆形的一部分。假设，已知椭圆形的长轴半径和短轴半径，另外已知两个点（它们的距离在圆的半径范围内），这时我们会发现，有两个路径可以连接这两个点。每种情况都可以生成出四种弧形。所以，为了保证创建的弧形唯一，A命令需要用到比较多的参数.

```html
A rx ry x-axis-rotation large-arc-flag sweep-flag x y
```

弧形命令A的前两个参数分别是x轴半径和y轴半径，它们的作用很明显，不用多做解释，如果你不是很清楚它们的作用，可以参考一下椭圆ellipse命令中的相同参数。弧形命令A的第三个参数表示弧形的旋转情况.


large-arc-flag（角度大小） 和sweep-flag（弧线方向），large-arc-flag决定弧线是大于还是小于180度，0表示小角度弧，1表示大角度弧。sweep-flag表示弧线的方向，0表示从起点到终点沿逆时针画弧，1表示从起点到终点沿顺时针画弧


## 基础变形

**g元素**

你可以把属性赋给一整个元素集合。实际上，这是它唯一的目的.

```html
<g fill="red">
  <rect x="0" y="0" width="10" height="10" />
  <rect x="20" y="0" width="10" height="10" />
</g>
```

**平移**

```
<rect x="0" y="0" width="10" height="10" transform="translate(30,40)" />
```

**旋转**

```
<rect x="20" y="20" width="20" height="20" transform="rotate(45)" />
```

**斜切**

skewX()  skewY()

**缩放**

scale(x,y)


## 剪切与遮罩


**剪切**

Clipping用于移除在别处定义的元素的内容.

```html
  <defs>
    <clipPath id="cut-off-bottom">
      <rect x="0" y="0" width="200" height="100" />
    </clipPath>
  </defs>

  <circle cx="100" cy="100" r="100" clip-path="url(#cut-off-bottom)" />
```

**遮罩**

遮罩的效果最令人印象深刻的是表现为一个渐变。如果你想要让一个元素淡出，你可以利用遮罩效果实现这一点.

```html
  <defs>
    <linearGradient id="Gradient">
      <stop offset="0" stop-color="white" stop-opacity="0" />
      <stop offset="1" stop-color="white" stop-opacity="1" />
    </linearGradient>
    <mask id="Mask">
      <rect x="0" y="0" width="200" height="200" fill="url(#Gradient)"  />
    </mask>
  </defs>

  <rect x="0" y="0" width="200" height="200" fill="green" />
  <rect x="0" y="0" width="200" height="200" fill="red" mask="url(#Mask)" />
```











