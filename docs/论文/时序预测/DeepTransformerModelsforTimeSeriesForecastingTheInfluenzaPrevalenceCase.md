# Deep Transformer Models for Time Series Forecasting: The Influenza Prevalence Case
[TOC]

[TOC]

| 阅读时间 | 2023-11-25 |
| -------- | ---------- |

## 论文主要内容

- 比较了基于传统方法、循环神经网络、`seq2seq`以及transformer模型在美国流感周度发病率的单步时间预测上的表现。

## 收获

- 了解到了`ARIMA`类的模型属于状态空间（`SSM`）模型；
- 这里transformer做时序预测时decoder的输入$\rightarrow$==decoder第一个输入是encoder最后一个输入==