---
title: Python matplotlib库使用
categories:
  - blog
tags:
  - python
  - 画图
excerpt: 记录一下python中的matplotlib库的使用方法。
usemathjax: true
---
# 前言
&emsp;因为日常学习生活中需要经常处理数据，一直有用python画图的需求，因此在此记录下python中常用的画图库——matplotlib的一些使用方法和细节。  
&emsp;废话不多说，让我们开始。
## 主要流程
&emsp;首先说一句废话，画图首先要有数据。在机器学习中，为了观察高维数据的分布情况，通常会使用t-SNE或者其他方法先将高维数据进行降维（一般来说降到2维），然后将2维数据画出来进行观察。因此这里使用2维数据为例子进行说明。  
&emsp;使用matplotlib库画图的主要程序框架如下：  
```
import matplotlib.plot as plt

def plot(which_fig, x, y, x_label='x', y_label='y', title='title', style_list=None):
  plt.figure(which_fig) #建图
  plt.clf*() #清空当前图
  plt.scatter(x, y, c = 'color', marker = '.') #x为横轴数据，y为纵轴数据，c为颜色代码，marker为点的形式
  plt.title(title) #设置图的标题
  plt.xlabel(x_label) #设置x轴名称
  plt.ylabel(y_label) #设置y轴名称
  plt.show() #显示图片
```
&emsp;整体的流程框架十分清楚，重点在于按照自己的需求订制绘图的相关细节，包括点的形式、颜色等等，下面针对这些进行详细叙述。  
## 参数订制
&emsp;matplotlib库常用的绘图函数有两个：一是 **plot()** ；二是 **scatter()**。通常来说，plot主要用于绘制经过点的曲线，scatter则用来绘制散点，因此，在使用的时候，要根据自己的绘图的需求选择合理的函数。  
&emsp;下面将详细介绍一下 **scatter()** 函数。首先是官方的文档定义：
```
matplotlib.pyplot.scatter(x, y, s=None, c=None, marker=None, cmap=None, norm=None, vmin=None, vmax=None, alpha=None, linewidths=None, verts=None, edgecolors=None, *, plotnonfinite=False, data=None, **kwargs)

A scatter plot of y vs x with varying marker size and/or color.

Parameter:
  x, y : array_like, shape (n, )
    The data positions.

  s : scalar or array_like, shape (n, ), optional
    The marker size in points**2. Default is rcParams['lines.markersize'] ** 2.

  c : color, sequence, or sequence of color, optional
    The marker color. Possible values:

      · single color format string.
      · sequence of color specifications of length n.
      · sequence of n numbers to be mapped to colors using cmap and norm.
      · 2-D array in which the rows are RGB or RGBA.
    Note that c should not be a single numeric RGB or RGBA sequence because that is indistinguishable from an array of values to be colormapped. If you want to specify the same RGB or RGBA value for all points, use a 2-D array with a single row. Otherwise, value- matching will have precedence in case of a size matching with x and y.

    Defaults to None. In that case the marker color is determined by the value of color, facecolor or facecolors. In case those are not specified or None, the marker color is determined by the next color of the Axes' current "shape and fill" color cycle. This cycle defaults to rcParams["axes.prop_cycle"] = cycler('color', ['#1f77b4', '#ff7f0e', '#2ca02c', '#d62728', '#9467bd', '#8c564b', '#e377c2', '#7f7f7f', '#bcbd22', '#17becf']).

  marker : MarkerStyle, optional
    The marker style. marker can be either an instance of the class or the text shorthand for a particular marker. Defaults to None, in which case it takes the value of rcParams["scatter.marker"] = 'o' = 'o'. See markers for more information about marker styles.

  cmap : Colormap, optional, default: None
    A Colormap instance or registered colormap name. cmap is only used if c is an array of floats. If None, defaults to rc image.cmap.

  norm : Normalize, optional, default: None
    A Normalize instance is used to scale luminance data to 0, 1. norm is only used if c is an array of floats. If None, use the default colors.Normalize.

  vmin, vmax : scalar, optional, default: None
    vmin and vmax are used in conjunction with norm to normalize luminance data. If None, the respective min and max of the color array is used. vmin and vmax are ignored if you pass a norm instance.

  alpha : scalar, optional, default: None
    The alpha blending value, between 0 (transparent) and 1 (opaque).

  linewidths : scalar or array_like, optional, default: None
    The linewidth of the marker edges. Note: The default edgecolors is 'face'. You may want to change this as well. If None, defaults to rcParams lines.linewidth.

  edgecolors : {'face', 'none', None} or color or sequence of color, optional.
    The edge color of the marker. Possible values:

      · 'face': The edge color will always be the same as the face color.
      · 'none': No patch boundary will be drawn.
      ·  A Matplotlib color or sequence of color.
    Defaults to None, in which case it takes the value of rcParams["scatter.edgecolors"] = 'face' = 'face'.

    For non-filled markers, the edgecolors kwarg is ignored and forced to 'face' internally.

  plotnonfinite : boolean, optional, default: False
    Set to plot points with nonfinite c, in conjunction with set_bad.
```
&emsp;乍一看，参数真多。但是日常用到的基本只有最常用的那几个。首先是数据点的坐标 **x** 和 **y**，然后是点的颜色 **c** 和图上点的形状 **marker**。
### 颜色
matplotlib颜色的指定有多种途径，具体如下：
1. 一个代表RGB或者RGBA通道的元组，元组元素的值范围为[0, 1]，例如：(0.1, 0.2, 0.5)或者(0.1, 0.2, 0.5, 0.3)；
2. 一个代表RGB或者RGBA模式颜色的字符串，例如：'#ofofof'或者'#ofofof80'。
3. 一个代表灰度强度的字符串，例如：'0.5'；
4. {'b', 'g', 'r', 'c', 'm', 'y', 'k', 'w'}中的一个，分别代表blue、green、red、cyan、mediumorchid(?)、yellow、black、white；
5. 一个 X11/CSS4 颜色名称；
6. xkcd色彩名称，以'xkcd:'打头，具体可查看(https://xkcd.com/color/rgb/)；
7. one of the Tableau Colors from the 'T10' categorical palette；
8. a 'CN' color spec。  

后两种方式基本用不到，在此就不做说明了，若想要了解具体的信息，可以到[这里](https://matplotlib.org/tutorials/colors/colors.html#sphx-glr-tutorials-colors-colors-py)仔细查看。
### 形状
确定好每个点的颜色后，下一步要做的就是确定每个点的表示形状。默认情况下， **marker** 变量为'o'，也就是'circle'。matplotlib库支持的形状较多，以下是详细的说明：


|缩写|含义|
|:-:|:-:|
|'.'|'point'|
|'o'|'circle'|
|'v'|'triangle_down'|
|'^'|'triangle_up'|
|'<'|'triangle_left'|
|'>'|'triangle_right'|


以上是常用的形状的缩写和其含义，更多的详细的信息可到[这里](https://matplotlib.org/api/_as_gen/matplotlib.markers.MarkerStyle.html#matplotlib.markers.MarkerStyle)查看。
