# Pytorch初始化

[TOC]

## 均匀xavier初始化

- 对输入张量执行均匀xavier初始化
- 用法：torch.nn.init.xavier_uniform_(tensor, gain=1.0)
- 常见用法：
  - 对某一个Layer的参数执行初始化