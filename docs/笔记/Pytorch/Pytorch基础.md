# Pytorch基础

[TOC]

## 常用操作符

### squeeze和unsqueeze

- squeeze用于将tensor中长度为1的维度删去；unsqueeze用于将tensor扩充一个长度为1的维度
- 用法：
  - `torch.squeeze(input, dim=None)`: torch.squeeze(input)将input中所有长度为1的维度去掉，torch.squeeze(input,dim)，如果dim所在维度的长度为1，则去掉dim维度，否则不做改变。
  - `Tensor.squeeze(dim=None)`
  - `torch.unsqueeze(input, dim)`：在dim维度上增加一个长度为1的维度
  - `Tensor.unsqueeze(dim)`

