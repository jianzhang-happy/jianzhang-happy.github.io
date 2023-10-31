# Pytorch基础

[TOC]

## 常用函数/方法

### squeeze和unsqueeze

- squeeze用于将tensor中长度为1的维度删去；unsqueeze用于将tensor扩充一个长度为1的维度
- 用法：
  - `torch.squeeze(input, dim=None)`: torch.squeeze(input)将input中所有长度为1的维度去掉，torch.squeeze(input,dim)，如果dim所在维度的长度为1，则去掉dim维度，否则不做改变。
  - `Tensor.squeeze(dim=None)`
  - `torch.unsqueeze(input, dim)`：在dim维度上增加一个长度为1的维度
  - `Tensor.unsqueeze(dim)`

### arange

- 类似于`np.arange`,返回一个一维张量
- 用法：
  - torch.arange(start=0, end, step=1, *, out=None, dtype=None, layout=torch.strided, device=None, requires_grad=False) → Tensor

### normal

- 产生正态分布随机数张量
- 用法：
  - torch.arange(*start=0*, *end*, *step=1*, *, *out=None*, *dtype=None*, *layout=torch.strided*, *device=None*, *requires_grad=False*) → [Tensor](https://pytorch.org/docs/stable/tensors.html#torch.Tensor)
  - 也可以指定形状参数size：torch.normal(*mean*, *std*, *size*, *, *out=None*) → [Tensor](https://pytorch.org/docs/stable/tensors.html#torch.Tensor) 

