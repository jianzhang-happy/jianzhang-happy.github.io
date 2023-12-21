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

### torch.triu

- 功能：对矩阵的最后两个维度取其上三角矩阵，其中`diagonal`参数控制对角线的位置（主对角线由数值更小的维度决定），此参数只能为整数，当为0时表示取包括主对角线以上元素，为1表示不包括主对角线，为-1表示取主对角线再往左下一行对角线，以此类推。
- 调用：`torch.triu(input, diagonal=0, *, out=None)`

### Tensor.masked_fill_

- 功能：将矩阵中指定位置填充指定值value，==当mask矩阵维度不足时，将运用广播机制==
- 调用：`Tensor.masked_fill_(mask, value)`

## 其他

### None用于tensor索引

None出现在索引中等价于在None所在维度将tensor增加一个维度，`tensor[None, :]`指在第0个维度将张量扩展一个维度。
