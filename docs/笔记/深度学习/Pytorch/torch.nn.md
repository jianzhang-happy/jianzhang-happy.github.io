# torch.nn

[TOC]

## 卷积层（[Convolution Layers](https://pytorch.org/docs/stable/nn.html#id1)）

### 不同维度卷积层比较

- nn.Conv1d和nn.Conv2d的输入参数基本都一样，基本包括输入通道数`in_channels`，输出通道数`out_channels`，卷积核大小`kernel_size`，步幅`stride`，填充`padding`，膨胀`dilation`。参数含义如下：

  - `in_channels`：输入通道数目

  - `out_channels`：输出通道数目

  - `kernel_size`：卷积核大小

  - `stride`：步幅大小

  - `padding`：填充数目；`padding`参数支持整数、元组和字符串输入，其中字符串输入有`valid`和`same`两种选项，`valid`表示无填充，`same`表示填充到和输入形状相同，==需要注意的是采用字符串输入时不支持`stride`不等于1==。

  - `dilation`：卷积核中用于计算的元素之间的间隔，默认间隔为1（也就是所有元素都参与运算）

  - `groups`：控制输入输出的联系，`groups=1`表示每个输入通道都将用于计算输出；`groups=2`表示一半的输入通道用于计算一半的输出通道，另一半输入通道计算另一半输出通道；

  - `bias`：是否添加bias

  - `padding_mode`：填充方式：`'zeros'`, `'reflect'`, `'replicate'` ,`'circular'`, 默认 `'zeros'

  - `device`：数据存储装置

  - `dtype`：数据类型

- 一维卷积(`nn.Conv1d`)和二维卷积(`nn.Conv2d`)的区别：

  - 一维卷积`kernel_size`形状为$1 \times kernel_{size}$，二维卷积核大小为$kernel_{size} \times kernel_{size}$或者$kernel_{size}[0] \times kernel_{size}[1]$；体现到参数上一维卷积`kernel_size`只能为整数/只有一个元素的tuple；

  - 一维卷积的`stride`参数同样也只能为整数/只有一个元素的tuple；

  - 一维卷积`padding`表示左右分别填充的长度，二维卷积则表示`(H,W)`这两个维度分别填充的维度

## 汇聚层（[Pooling layers](https://pytorch.org/docs/stable/nn.html#id1)）

### 不同维度汇聚层比较

- 最大汇聚层：一维最大汇聚层和二维最大汇聚层参数相同，包括：

  - `kernel_size`，`stride`，`padding`，`dilation`：对于一维，只能为整数或者包含一个整数的tuple；对于二维，可以为整数或者包含两个整数的tuple

  - `return_indices`：除了返回最大值，还会返回最大值对应index

  - `ceil_mode`：将会使用ceil而不是floor计算输出形状

- 平均汇聚层：二维平均汇聚层比一维平均汇聚层多一个`divisor_override`参数，相同参数如下：
  - `kernel_size`，`stride`，`padding`：对于一维，只能为整数或者包含一个整数的tuple；对于二维，可以为整数或者包含两个整数的tuple；其中`padding`采用0填充；
  - `ceil_mode`：将会使用ceil而不是floor计算输出形状
  - `count_include_pad`：计算均值时是否纳入填充的0
  - `divisor_override`：整数；指定用于计算均值的分母

## 损失函数

### 概述

损失函数类都是nn.Module的子类。

### nn.CrossEntropy

- 常见调用方式`loss=nn.CrossEntropy(reduction='None')`，reduction参数默认为mean，可选mean、none、sum
- 输入是概率(即softmax输出的概率)或者没有经过softmax转换的'概率'，目标既可以为概率，也可以是[0, n)之间的类别标号。==其中采用类别标号计算会得到优化==
