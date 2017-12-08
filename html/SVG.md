# SVG


## 概念

### 全称：可缩放矢量图形

### 用途：使用 XML 来描述二位维图形和绘图程序的语言

### 特点：矢量

## 嵌入方式

### 通过 embed 标签

### 通过 object 标签

### 通过 iframe 标签

### 直接使用 svg 标签

## 预定形状

### 矩形：rect

#### x/y：图形位置

#### width/height

#### rx/ry：矩形圆角

### 圆形：circle

#### cx/cy：圆心位置

#### r：半径

### 椭圆：ellipse

#### cx/cy：圆心位置

#### rx/ry：水平/垂直半径

### 线：line

#### x1/y1：线条开始位置

#### x2/y2：线条结束位置

### 折线：polyline

#### points：定义多边形每个的 x/y轴坐标

### 多边形：polygon

### 路径：path

#### d

- M：moveto 

- H：horizontal lineto

- V：vertical lineto

- ：curveto

- S：smoothto

- L：lineto

- Q：quadratic Belzier curve

- T：smooth quadratic Belzier curveto

- A：elliptical Arc

- Z：closepath

#### 以上所有命令均允许小写字母。大写表示绝对定位，小写表示相对定位。

## 通用属性

### style

#### fill：填充颜色

#### stroke：线条颜色

#### stroke-width：线条粗细

#### opacity

#### fill-opacity

#### stroke-opacity

## 滤镜

### 高斯滤镜：<filter>

#### 必需嵌在<defs>标签内

#### id：必需，应用该滤镜时使用的名称

#### filter:url(#id)：指向要应用的滤镜（位于其他标签上）

#### <feGaussianBlur>：定义滤镜效果

- <feGaussianBlur> 标签的 stdDeviation 属性可定义模糊的程度

- in="SourceGraphic" 这个部分定义了由整个图像创建效果

## 渐变

### <linearGradient>：线性渐变

#### 必需嵌在<defs>标签内

#### id：必需，应用该渐变时使用的名称

#### fill:url(#id)：指向要应用的渐变（位于其他标签上）

#### <linearGradient> 标签的 x1、x2、y1、y2 属性可定义渐变的开始和结束位置

#### 渐变的颜色范围可由两种或多种颜色组成。每种颜色通过一个 <stop> 标签来规定。offset 属性用来定义渐变的开始和结束位置。

### <radialGradient>：放射渐变

#### 必需嵌在<defs>标签内

#### id：必需，应用该渐变时使用的名称

#### fill:url(#id)：指向要应用的渐变（位于其他标签上）

#### cx、cy 和 r 属性定义外圈，而 fx 和 fy 定义内圈 渐变的颜色范围可由两种或多种颜色组成。每种颜色通过一个 <stop> 标签来规定。offset 属性用来定义渐变的开始和结束位置。

