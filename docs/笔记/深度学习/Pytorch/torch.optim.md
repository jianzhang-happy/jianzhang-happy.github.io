# `torch.optim`

[TOC]

## 概述

`optim`实现了多种优化器，其中优化器的构造方式主要有两种：

- 第一种方式所有参数的更新方式相同（即学习率，decay_rate等相同），构造方式如：

  ```python
  optimizer = optim.SGD(model.parameters(), lr=0.01, momentum=0.9)
  optimizer = optim.Adam([var1, var2], lr=0.0001)
  ```

- `optim`也支持不同参数采用不同的更新方式，这种构造方式传入字典，其中字典中未指定的参数共享字典之外的关键词参数，如：

  ```python
  optim.SGD([
                  {'params': model.base.parameters()},
                  {'params': model.classifier.parameters(), 'lr': 1e-3}
              ], lr=1e-2, momentum=0.9)
  # classifier.parameters()的学习率为1e-3,其他参数学习率为1e-2
  ```

## `torch.optim.SGD`

### 类声明：

> `torch.optim.SGD(params, lr=<required parameter>, momentum=0, dampening=0, weight_decay=0, nesterov=False, *, maximize=False, foreach=None, differentiable=False)`

