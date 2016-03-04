# Draw方言
------
## 简介

**Draw**方言用简单声明的方式提供了2D绘图操作。此类操作用值块表示命令序列清单(称为绘制块)，可在运行时自由构造与更改。
绘制块可用 `draw` 函数直接在图片上渲染，或用在`view face` 的 `draw facet` 中。

## 命令

命令由绘图指令及模式设置构成。当一个模式被设置，将影响当前会话中所有后序操作直到被改变。
多数绘图命令需要指定坐标。部分命令需要指定长度值(以像素为单位)。

X轴：数值从屏幕左侧向右侧增大；
Y轴：数值从屏幕顶部向底部增大；
![坐标轴](http://i.imgur.com/BzeTBmP.png)

### 基本绘图命令

#### 线

    line <point> <point> …
	     <point>:点的坐标值(pair!)
在两点间画线。若指定了多个点，则按顺序两两相连。

#### 三角形
    triangle <point> <point> <point>
指定三个顶点，绘制三角形。

#### 矩形
    box <top-left> <bottom-right>
    box <top-left> <bottom-right> <corner>
由左上角和右下角顶点画矩形。`<corner>`设定转角弧度。

#### 多边形
    polygon <point> <point>  <point> …
指示顶点，画多边形。至少有三个点。

#### 圆
    circle <center> <radius>
    circle <center> <radius-x> <radius-y>
指定圆心及半径以画圆。若指定了两个半径就是椭圆模式。

#### 椭圆
    ellipse <center> <radius>
由圆心及椭圆半径画椭圆。椭圆半径由X轴及Y轴半径组成的`pair!`值构成。
是用 `circle` 命令画椭圆的便捷方式。

#### 弧形
    arc <center> <radius> <begin> <end>
    arc <center> <radius> <begin> <end> closed
指定圆心，半径及起止角度，画弧形。
关键字 `closed` 连接弧形两端与圆心，成为扇形。

#### 贝兹曲线
    curve <end-A> <control-A> <end-B>
    curve <end-A> <control-A> <control-B> <end-B>
由3个以上的控制点画贝塞尔曲线。
				
#### B样曲线
    spline <point> <point> ...
    spline <point> <point> ... closed
由三个以上的点画B样曲线，关键字 `closed` 使曲线封闭。若仅提供两个点就画成直线。

#### 图片
    image <image>
    image <image> <top-left>
    image <image> <top-left> <bottom-right>
    image <image> <top-left> <top-right> <bottom-left> <bottom-right>
    image <image> <top-left> <top-right> <bottom-left> <bottom-right> <color>
    image <image> <top-left> <top-right> <bottom-left> <bottom-right> <color> border
四点及`border`模式还没有实现。
这个命令主要是对图片涂色。<color>`可设定透明度。

#### 文本
    text <position> <string>
在指定位置显示文本。
如果`font-color`为`none`，就使用`pen-color`。

### 属性设置命令：

#### 字体
    font <font>
设置文本字体，<font>是font!对象的克隆。

#### 线条色
    pen <color>
设置画笔颜色。

#### 填充色
    fill-pen <color>
    fill-pen off
所有封闭形状的填充色。off表示关闭填充功能。

#### 线宽
    line-width <value>
设置线的宽度。

####连线模式
    line-join <mode>
设置线的连接模式。
![连线模式图](https://github.com/red/red/wiki/images/line-join.png)
`miter`是默认模式。
还有种`miter-bevel`模式依据斜接长度自动选择。

#### 线的端点模式
    line-cap <mode>
设置线的端点模式。默认为flat.
![端点模式](https://github.com/red/red/wiki/images/line-cap.png)

#### 平滑模式
    anti-alias <mode>
开关反锐化处理。

##默认值

当一个新绘图会话开始，将使用下列默认值：

|属性           |值      |
|----           |----   |
|**background** |white  |
|**pen color**  |black  |
|**filling**    |off    |
|**anti-alias** |on     |
|**font**		|none   |
|**line width** |1      |
|**line join**  |miter  |
|**line cap**   |flat   |

## 子块
在绘图代码内，命令可用块任意组合，而不改变语义。这样方便对命令分组操作，如提取，插入，删除等。同时，接受空块。

## 源位置
`set-word!`可用在命令之间以记录当前位置，被后续命令使用。
如果`set-word!`之前的命令有变动，它所记录的位置并不会更新。

## Draw函数
	draw <size> <spec>
	draw <image> <spec>
可直接使用draw函数在图片上呈现绘制。然后返回被绘制的图片。

## 本文用图的绘制源码
	Red [
	    Title:  "Graphics generator for Draw documentation"
	    Author: "Nenad Rakocevic"
	    File:   %draw-graphics.red
	    Needs:  View
	]
	
	Arial: make font! [name: "Consolas" style: 'bold]
	small: make font! [size: 9 name: "Consolas" style: 'bold]
	
	save %line-cap.png draw 240x240 [
	    font Arial
	    text 20x220  "Flat"
	    text 90x220  "Square"
	    text 180x220 "Round"
	
	    line-width 20 pen gray
	    line-cap flat   line 40x40  40x200
	    line-cap square line 120x40 120x200
	    line-cap round  line 200x40 200x200
	
	    line-width 1 pen black
	    line 20x40  220x40
	    line 20x200 220x200
	]
	
	save %line-join.png draw 500x100 [
	    font Arial
	    text 10x20  "Miter"
	    text 170x20 "Round"
	    text 330x20 "Bevel"
	
	    line-width 20 pen gray
	    line-join miter line 140x20 40x80  140x80
	    line-join round line 300x20 200x80 300x80
	    line-join bevel line 460x20 360x80 460x80
	
	    line-join miter
	    line-width 1 pen black
	    line 140x20 40x80  140x80
	    line 300x20 200x80 300x80
	    line 460x20 360x80 460x80
	]
	
	save %coord-system.png draw 240x240 [
	    font small
	    text 5x5 "0x0"
	    line-width 2
	    line 20x20 200x20 195x16
	    line 200x20 195x24
	
	    line 20x20 20x200 16x195
	    line 20x200 24x195
	
	    font Arial
	    text 205x12 "X"
	    text 12x205 "Y"
	]
