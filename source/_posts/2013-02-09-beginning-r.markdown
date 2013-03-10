---
layout: post
title: "R初略学习笔记"
date: 2013-02-09 14:18
comments: true
tags: 
- R 
- Statistics
- DataVisualization
---
## 包管理

从CRAN安装一个包：`install.packages("pkg_name")`

加载包：`library(package)`

## R的基本数据类型

在R里面一个元素的基本数据类型称为 mode。每个对象都有mode属性。

基本数据类型包括：数值(实数) numeric，复数 complex，字符(串) character，逻辑(布尔) logical 和 raw。NA表示没有值。

基本数据类型之间的转换通过 as.*something*() 函数进行，例如 `as.character()`, `as.integer()`。

## R的数据对象类型

### 向量 vector

向量由一组相同类型的元素组成。

用函数 c() 可以构造出向量：

	x <- c(10.4, 5.6, 3.1, 21.7)


如果要生成元素为序列的向量，简单的方法是用冒号，如 1:7，复杂的方法是函数 seq()。另外，函数 rep() 可以根据已有向量的元素重复来生成新向量。

向量中的某个元素通过 [] 访问，如 `x[3]`，注意，下标从1开始！方括号中下标位置放的可以是向量，称为索引向量 index vector，这时候会根据索引取出特定元素构成新的向量。例如 `x[1:10]` 取出x的前10个元素。

对向量做运算，是对向量的每个元素做独立运算，结果是一个相同元素个数的新向量。

### 因子 factor

**TODO**：没懂

### 数组 array

数组是多维的一组相同类型数据。特别地，二维数组称为矩阵 matrix。

通过指定维度属性 dim，向量可以转换为数组。例如z为有1500个元素的向量，`dim(z) <- c(3, 5, 100)` 将z变作3*5*100的三维数组。另外一种构造数组的方法是用函数 array()： `Z <- array(data_vector, dim_vector)`。向量中元素是按照首下标优先的顺序来遍历的，如 `z[1,1,1], z[2,1,1], …, z[2,5,100], z[3,5,100]`。

可以通过 `a[2,,]` 这样的方法访问子数组，得到的是指定了第一维为2后剩余维数的数组。正如可以用向量为下标来访问向量，也可以用索引矩阵 index matrix 作为下标来访问数组，这样可以根据索引矩阵的值为下标取出数组中的特定一组元素。

通过 `as.vector(X)` 或者 `c(X)` 可以将数组X变回向量。

### 列表 list

文档里叫list，但实际上更象其他语言里的map，或者叫hash。列表里的每个元素是一个对象，称为组件 componet，列表里的组件的类型不需要一致。

生成列表的方法：`Lst <- list(name_1=object_1, …, name_m=object_m)`。组件名字是可忽略的。构成列表的对象会复制一个副本到列表里面，而不是引用。

用 `[[n]]` 来访问列表中的第n个组件。如果组件有名字，用 `list$component_name` 的方法更简便。组件名可以简写为名字的前几个字母，只要不引起歧义就行。

### 数据帧 data frame

数据帧是特殊的列表：

- 组件为向量(数值型,字符形,逻辑型)，因子，数值矩阵，列表或其他数据帧
- 数据帧中作为变量的向量结构必须具有相同的长度,而矩阵结构应当具有相同的行大小

很多情况下,数据帧会被当作各列具有不同模式和属性的矩阵。

数据帧同样可以用下标来获取子集，例如看前三行数据：`data[1:3,]`，根据某列的值做过滤：`filtered_frame <- full_frame[full_frame$country=="cn",]`

通过 `as.matrix(X)` 将数据帧转换为矩阵。

## 读取数据

函数 `read.table()` 读取文本文件的数据，产生一个数据帧。更常用的是 `read.csv()` 和 `read.delim()`，它们是 read.table() 的缺省参数版本，后者读取tab分隔的文件。文件的每个列被保存到数据帧的一个组件中，组件名字就是列的名字，如果列名里有空格会被转换成句点。

## 绘图

### 高级绘图函数

下面是R中高级绘图函数的概括:

函数 | 说明 |
:----|:-----|
plot(x) |以x的元素值为纵坐标、以序号为横坐标绘图 |
plot(x, y) | x(在x-轴上)与y(在y-轴上)的二元作图。根据type参数可以画出多种类型的图。|
sunflowerplot(x,y) | 同上 但是以相似坐标的点作为花朵,其花瓣数目为点的个数 |
pie(x) | 饼图 |
boxplot(x) | 盒形图(“box-and-whiskers”) |
stripchart(x) | 把x的值画在一条线段上,样本量较小时可作为盒形图的替代 |
coplot(x~y\|z) | 关于z的每个数值(或数值区间)绘制x与y的二元图 |
interaction.plot(f1, f2, y) | 如果f1和f2是因子,作y的均值图,以f1的不同值作为x轴, 而f2的不同值对应不同曲线;可以用选项fun指定y的其他的统计量(缺省计算均值,fun=mean) |
matplot(x,y) | 二元图,其中x的第一列对应y的第一列,x的第二列对应y的第二 列,依次类推。 |
dotchart(x) | 如果x是数据框,作Cleveland点图(逐行逐列累加图) |
fourfoldplot(x) | 用四个四分之一圆显示2X2列联表情况(x必须是dim=c(2, 2, k)的数组,或者是dim=c(2, 2)的矩阵,如果k = 1) | 
assocplot(x) | Cohen–Friendly图,显示在二维列联表中行、列变量偏离独立性 的程度 | 
mosaicplot(x) | 列联表的对数线性回归残差的马赛克图 | 
pairs(x) | 如果x是矩阵或是数据框,作x的各列之间的二元图 | 
plot.ts(x) | 如果x是类"ts"的对象,作x的时间序列曲线,x可以是多元的, 但是序列必须有相同的频率和时间 | 
ts.plot(x) | 同上,但如果x是多元的,序列可有不同的时间但须有相同的频 率 | 
hist(x) | x的频率直方图 | 
barplot(x) | x的值的条形图 | 
qqnorm(x) | 正态分位数-分位数图 | 
qqplot(x, y) | y对x的分位数-分位数图 | 
contour(x, y, z) | 等高线图(画曲线时用内插补充空白的值),x和y必须为向量 ,z必须为矩阵 , 使得dim(z)=c(length(x), length(y))(x和y可以省略) | 
filled.contour (x,y, z) | 同上,等高线之间的区域是彩色的,并且绘制彩色对应的值的图 例 | 
image(x, y, z) | 同上,但是实际数据大小用不同色彩表示 | 
persp(x, y, z) | 同上,但为透视图 | 
stars(x) | 如果x是矩阵或者数据框,用星形和线段画出 | 
symbols(x, y, ...) | 在由x和y给定坐标画符号(圆,正方形,长方形,星,温度计式 或者盒形图),符号的类型、大小、颜色等由另外的变量指定 |
termplot(mod.obj) | 回归模型(mod.obj)的(偏)影响图 |
heatmap(x) | 热度图 |

#### 需要工具包支持的高级绘图

**板块层级图 tree map**

用矩形面积来表示数值，可用于分析磁盘空间占用

	library(portfolio)
	map.market(id, area, group, color)

**平行坐标图**

	library(lattice)
	parallel(data)

### 低级绘图命令

R里面有一套绘图函数是作用于现存的图形上的:称为低级作图命 令(low-level plotting commands)。下面有一些主要的:

函数 | 说明 |
:----|:-----|
scatter.smooth(x, y, ...) | LOESS(局部加权散点平滑)拟合曲线 | 
points(x, y) | 添加点(可以使用选项type=) | 
lines(x, y) | 同上,但是添加线 | 
text(x, y, labels,...) | 在(x,y)处添加用labels指定的文字;典型的用法是: plot(x, y, type="n"); text(x, y, names) | 
mtext(text,side=3, line=0,...) | 在边空添加用text指定的文字,用side指定添加到哪一边(参照 下面的axis());line指定添加的文字距离绘图区域的行数 | 
segments(x0, y0,x1, y1) | 从(x0,y0)各点到(x1,y1)各点画线段 | 
arrows(x0, y0,x1, y1, angle= 30,code=2) | 同上但加画箭头,如果code=2则在各(x0,y0)处画箭头,如 果code=1则在各(x1,y1)处画箭头,如果code=3则在两端都画箭头; angle控制箭头轴到箭头边的角度 | 
abline(a,b) | 绘制斜率为b和截距为a的直线 | 
abline(h=y) | 在纵坐标y处画水平线 | 
abline(v=x) | 在横坐标x处画垂直线 | 
abline(lm.obj) | 画由lm.obj确定的回归线(参照第五章) | 
rect(x1, y1, x2, y2) | 绘制长方形,(x1, y1)为左下角,(x2,y2)为右上角 | 
polygon(x, y) | 绘制连接各x,y坐标确定的点的多边形 | 
legend(x, y, legend) | 在点(x,y)处添加图例,说明内容由legend给定 | 
title() | 添加标题,也可添加一个副标题 | 
axis(side, vect) | 画坐标轴,side=1时画在下边,side=2时画在左边,side=3时画在上边,side=4时画在右边。可选参数at指定画刻度线的位置坐标 | 
box() | 在当前的图上加上边框 | 
rug(x) | 在x-轴上用短线画出x数据的位置 | 
locator(n, type="n", ...) | 在用户用鼠标在图上点击n次后返回n次点击的坐标(x, y);并可以在点击处绘制符号(type="p"时)或连线(type="l"时),缺省情 况下不画符号或连线 | 

## Reference

- R for Beginners
- An Introduction to R

