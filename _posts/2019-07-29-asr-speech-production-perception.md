---
layout: post
title:  "语音产生和感知原理"
date:   2019-07-29 10:00:00 +0800
categories: asr
---
{: class="table-of-content"}
* TOC
{:toc}

**TODO**
* 增加图片

*注:本文是分享e6870课程lecture1的内部讲义，本文不介绍傅立叶分析，阅读前需要有频域的概念*

*什么是基础频率f0，什么是共振峰F1，F2，F3，什么是音高pitch，f0有什么用*
# 发音原理

### 声带

**谐波**
只有一个频率成分的波称为谐波，比如一个正弦波。 一个吉他上的一根弦产生的波可以认为是一个谐波。


**基础频率f0**

声带震动产生一组谐波(harmonic), 其中最小的频率是f0，其他谐波的频率是f0的整数倍。各谐波的振幅每8度音下降12db。

**pitch**

pitch是指一个声音人类感知到的音高，和f0不是一个东西，f0是发音相关概念，pitch是个听觉感知相关概念。

*测量pitch的方法:给定一个声音S，拿一个可以调整频率的谐波发生器，比如音叉或者乐器，不断调整该谐波发声器，人耳如果听起来和声音S发音的音高一致，则该谐波发生器当前的音高就是声音S的pitch。*

因为pitch不好计算，而pitch的变化和f0的变化是一致的，所以经常用f0 tracker代替pitch tracker.

在ASR中所谓的提取pitch特征常常指提取f0特征。

**f0的变化**

一般一个人正常说话的f0是基本不变得的，也可以故意降低或者升高，比如唱歌，故意捏着嗓门，故意低沉说话。

### 声道
声道相当于一个滤波器，对声带振动产生的信号做时域卷积，即在频域上做乘积，相当于把某些谐波放大，把某些谐波减小。
声道滤波器频域上的峰值（放大的频率成分）就是共振峰，F1，F2，F3，F4

图中

### 总结
* f0和其他谐波来自于声带震动，
* 共振峰来自于声道的形状。
* 不同震动频率的声带通过不同形状的声道，形成了各种声音。



### 语谱图

**语谱图的维度**
* 时间
* 频率
* 能量值

使用二维表示时用颜色深浅来表示能量值大小。

**怎么看语谱图**

* 整体能量分布，低频，中频，高频
* 亮细线，谐波。最低的是f0，亮细线之间的间隔也是f0.
* 粗线变化是共振峰。

用audition演示 bi bei ha na li yi


**辅音和元音**
* 元音有f0, 不同的元音的F1，F2共振峰的位置有明显差异。不同的发音器官位置带来了不同的共振峰位置，也会产生不同的声音。
* 辅音没有f0，声道不震动.

**F0的变化**

F0的变化会带来音调(tone)和语调(intonation)的变化。

下图是中文“bei”的四个音调的语谱图，从左到右第一声到第四声。


# 听觉原理

人耳内的basilar membrane hair相当于带通滤波器，感知某个频率范围的声音。

人耳对低频变化敏感，对高频的变化不敏感。举个例子，比如当声音从300Hz变换为400Hz时，人类能明显听出区别，
而当声音从2300Hz变换为2400Hz时，同样是增加了100Hz，人类分辨不出区别。

因此可以将频域做一个对数变换，变换后的空间称为mel域，在mel域上人耳对mel频率的分辨是线性变化的。

# 机器的“听觉”

机器也不是完美的记录了听到的声音。类似于人耳，有如下问题:

1. 传感器元器件的频域响应
1. 有限采样率，8K只能保留4K的信息
1. 16bit量化，损失了精度
1. 音频压缩
