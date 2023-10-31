# Pytorch常用Layer

[TOC]

## nn.linear类

- 对输入矩阵的==最后一个维度==进行线性变化
- 用法：torch.nn.Linear(*in_features*, *out_features*, *bias=True*, *device=None*, *dtype=None*)
- 参数解释：
  - in_features：输入维度
  - out_features：输出维度
  - bias：是否加偏置