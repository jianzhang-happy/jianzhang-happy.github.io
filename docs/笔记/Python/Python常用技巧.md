# Python常用技巧

[TOC]

## `time`和`timeit`计时

- `time`和`timeit`计时的区别在于前者只返回一次运行代码的时间，后者返回多次运行的时间的均值和标准差

- 在`jupyter`中可以使用`%time`和`%timeit`对当前行运行的代码计时，用`%%time`和`%%timeit`对当前单元计时

## pandas中rolling().apply()加速

- 对于pandas内置的rolling().mean()、rolling().std()等方法，内置函数速度很快
- 如果要运用rolling().apply(`func`)，当数据量较大的时候最好采用`numpy`进行运算