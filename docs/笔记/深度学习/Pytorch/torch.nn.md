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

## 循环层

### nn.LSTM

- 功能：
- 调用：`torch.nn.LSTM(self, input_size, hidden_size, num_layers=1, bias=True, batch_first=False, dropout=0.0, bidirectional=False, proj_size=0, device=None, dtype=None)`
- 参数：
  - input_size：输入序列的特征数目
  - hidden_size：隐藏层特征数目
  - num_layers：循环层层数
  - bias：计算时是否添加bias
  - batch_first：输入和输出序列的形状是否为$batch \times seq \times feature$，默认为否，即输入输出序列形状为$seq \times batch \times feature$
  - dropout：对于多层循环层网络，如果dropout非零，那么在除最后一层输出层上添加一个dropout层。
  - bidirectional：是否是双向循环网络，默认为否
  - proj_size：

- 输入：input，(h_0, c_0)
  - input：输入大小为$seq \times feature_{in}$（没有batch时）或者$seq \times batch \times feature_{in}$，如果batch_first参数为True的时候，输入形状为$batch \times seq \times feature$。
  - h_0、c_0：初始隐藏单元和记忆单元，对于单向网络，形状为$num_{layers} \times feature_{out}$，对于双向网络则为$2 \times num_{layers} \times feature_{out}$（没有batch时），当为batch输入时，则分别为$num_{layers} \times batch \times feature_{out}$和$2 \times num_{layers} \times batch \times feature_{out}$。如果没有指定，则h_0和c_0默认都是0元素。
- 输出：output，(h_n, c_n)
  - output：
  - h_n
  - c_n

## 损失函数

### 概述

损失函数类都是nn.Module的子类。

### nn.CrossEntropy

- 概述：常见调用方式`loss=nn.CrossEntropyLoss(reduction='None')`，reduction参数默认为mean，可选mean、none、sum；输入是概率(即softmax输出的概率)或者没有经过softmax转换的'概率'，目标既可以为概率，也可以是[0, n)之间的类别标号。==其中采用类别标号计算会得到优化==
- 

## 归一化层

### 概述

- 归一化的原理都是对输入数据进行标准化，遵循以下公式：

$$
y = \frac{x-E[x]}{\sqrt{Var[x]+\epsilon}} * \gamma + \beta
$$

不同的归一化方式在于均值和标准差的计算方式不同，对于LayerNorm，均值/标准差为在给定维度计算（对于LayerNorm，指定维度只能是最后一个维度），而BatchNorm则为在除给定维度之外的维度计算（对于BatchNorm1d和BatchNorm2d，指定维度只能是第二个维度）；

- 示例：

  ```python
  # NLP Example
  batch, sentence_length, embedding_dim = 4, 2, 3
  embedding = torch.arange(24, dtype=torch.float32).reshape(batch, sentence_length, embedding_dim)
  layer_norm = nn.LayerNorm(embedding_dim)
  a = layer_norm(embedding)
  
  b = (embedding-embedding.mean(dim=(-1),keepdim=True)) / torch.sqrt(embedding.var(dim=(-1),keepdim=True,unbiased=False) + 1e-5)
  # a,b的各个元素相同
  ```

  

### nn.BatchNorm1d

- 功能：对2维或者3维输入进行Batch归一化，均值/方差计算在给定维度之前的维度进行
- 需要注意的是在训练和验证阶段BatchNorm的行为不一样

### nn.BatchNorm2d

- 功能：对4维输入进行Batch归一化

### nn.LayerNorm

- 功能：对输入进行Layer归一化，均值/方差计算方式在给定维度进行
- 调用：`torch.nn.LayerNorm(normalized_shape, eps=1e-05, elementwise_affine=True, bias=True, device=None, dtype=None)`
- 参数：
  - normalized_shape：int/str/torch.Size; 此参数只能为输入形状的后几位数字组成的元组
  - eps：float； 计算时分母中的基准
  - elementwise_affine：bool；$\gamma$和$\beta$是否可以学习
  - bias：bool；是否学习偏差$\beta$

## dropout层

### nn.Dropout

- 功能：对输入进行dropout操作；在训练过程中，输入会乘以缩放因子$\frac{1}{1-p}$，而在验证阶段dropout不起作用（相当于乘以1）
- 调用：`torch.nn.Dropout(p=0.5, inplace=False)`
- 参数：
  - p：dropout概率
