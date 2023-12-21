# lightgbm自定义损失函数

[TOC]

本文介绍lightgbm自定义损失函数的方式，所使用的API版本为`4.1.0`，这里的介绍基于python API

## 基本介绍

- lightgbm自定义损失函数需要写两个函数，一个函数用于训练，此函数输入目标值和真实值（其中真实值的类别是lightgbm的Dataset类，需要用get_label方法得到真实值标签矩阵），返回损失函数关于目标值的一阶导数（gradient/grad）和二阶导数（hessian/hess）；第二个函数计算损失函数用于验证，此函数同样输入目标值和真实值，返回值包含三个，分别是损失名称，损失大小，是否期望更高的损失；
- 第一个函数作为lightgbm的参数"objective"输入，第二个函数作为train的fevl参数输入；
- 对于某些不容易求导的损失类型，可以借助pytorch的自动求导来实现；

## 示例

- 手动实现`l2`损失，需要注意的是如果损失函数形式上是对所有实例的损失进行求和或者求均值，其实两者结果是一致的，因为两者的导数只差了一个常数倍数关系。

  ```python
  def l2_loss(y, data):
      t = data.get_label()
      grad = (y - t) / len(y)
      hess = np.ones_like(y) / len(y)
      return grad, hess
  
  def l2_eval(y, data):
      t = data.get_label()
      loss = (y-t)**2
      return 'l2', loss.mean(), False
  ```

- 利用pytorch的自动求导实现`l2`损失

  ```python
  def l2_loss_torch(y, data):
      t = data.get_label()
      t = torch.tensor(t).reshape(shape=(len(t),))
      y = torch.tensor(y, requires_grad=True).reshape(t.shape)
      loss = ((y - t) ** 2).mean()
      gradient, = grad(loss, y, create_graph=True)
      hessian = []
      for i in range(len(y)):
          hessian_, = grad(gradient[i], y, retain_graph=True)
          hessian_ = hessian_[i].item()
          hessian.append(hessian_)
      return gradient.detach().numpy(), hessian
  
  def l2_eval(y, data):
      t = data.get_label()
      loss = (y-t)**2
      return 'l2', loss.mean(), False
  ```

  







