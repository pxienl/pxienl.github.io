---
title: 滤波算法
date: 2024-10-06 14:32:41
tags: embedded
---

+ 无限冲激响应（Infinite Impulse Response，简称 IIR）是一种数字滤波器的类型，其输出依赖于当前和过去的输入以及过去的输出。
+ 有限冲激响应（Finite Impulse Response，简称 FIR）是一种数字滤波器的类型。它的特点是响应时间有限，意味着滤波器的输出仅依赖于当前和之前有限数量的输入样本。

+ 数字信号处理（Digital signal processing，简称 DSP）一种利用数字计算方法对信号进行分析、修改和合成的技术。



# Median filter(中值滤波)

中值滤波器是一种非线性数字滤波技术，它具有可选择的窗口大小，窗口的大小会影响过滤能力和处理时间。通常可以用作更高级滤波器（例如卡尔曼滤波器）的预处理步骤。

这里以维基百科中 中值滤波器对一维信号处理为示例：

> To demonstrate, using a window size of three with one entry immediately preceding and following each entry, and zero-padded boundaries, a median filter will be applied to the following simple one-dimensional signal:
>
> ​	x = (2, 3, 80, 6, 2, 3).
>
> This signal has mainly small valued entries, except for one entry that is unusually high and considered to be a noise spike, and the aim is to eliminate it. So, the median filtered output signal y will be:
>
> ​	y0 = med(0, 2, 3) = 2, (the boundary value is taken to be 0)
> ​	y1 = med(2, 3, 80) = 3, (already 2, 3, and 80 are in the increasing order so no need to arrange them)
> ​	y2 = med(3, 80, 6) = med(3, 6, 80) = 6, (3, 80, and 6 are rearranged to find the median)
> ​	y3 = med(80, 6, 2) = med(2, 6, 80) = 6,
> ​	y4 = med(6, 2, 3) = med(2, 3, 6) = 3,
> ​	y5 = med(2, 3, 0) = med(0, 2, 3) = 2,
>
> i.e.,
>
> ​	y = (2, 3, 6, 6, 3, 2).
>
> 
>
> ref: https://en.wikipedia.org/wiki/Median_filter#Worked_two-dimensional_example



# Arithmetic Mean Filter(均值滤波)

均值滤波就是采样 n 个值进行平均值计算
$$
\bar{x} = \frac{1}{n} (x_1+ \cdots +x_n)
$$

## Moving Average Filter(移动均值滤波)

移动均值有多种形式，一般是指 简单移动均值滤波SMA

可参考: https://codemonk.io/blog/moving-average-filter/

### Simple Moving Average (SMA) 简单移动均值

> 移动平均滤波器（Moving Average Filter）原理，移动平均滤波基于统计规律，将连续的采样数据看成一个长度固定为N的队列，在新的一次测量后，上述队列的首数据去掉，其余N-1个数据依次前移，并将新的采样数据插入，作为新队列的尾；然后对这个队列进行算术运算，并将其结果做为本次测量的结果。
>
> ref: [移动平均滤波器_百度百科 (baidu.com)](https://baike.baidu.com/item/移动平均滤波器)

### Weighted Moving Average (WMA) 加权移动均值



### Exponentially Weighted Average (EWA) 指数加权均值

指数加权平均值对最近的数据点给予更多的权重，而对之前的数据点总体给予更少的权重。

示例参考： https://www.geogebra.org/m/tb88mqrm

```c
// output     EWA输出
// alpha      平滑系数 [0,1]
// reading    输入值
// lastOutput 上次的过滤输出值
output = alpha * reading + (1 - alpha) * lastOutput
// or
// alphaScale 平滑系数缩放比例
output = (alpha * reading + (alphaScale - alpha) * lastOutput) / alphaScale
```



待补充
