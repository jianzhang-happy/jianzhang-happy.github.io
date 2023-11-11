# lightgbm

[TOC]

本文介绍lightgbm的python API的使用

## :question:目前有疑问的点

### 可复现性的问题

在不指定lightgbm的seed和deterministic=True参数的情况下，用同样的参数在相同的数据集上训练得到的结果是一样的。目前看文档说的是需要指定deterministic=True的情况下结果才是确定的。默认deterministic=False。即使device不同（cpu和gpu），结果也一样。

### 运行效率

cpu相比gpu效率慢挺多的，cuda还没试过。

## :one: 用法baseline

## :two: 重要参数

## :three: 调参baseline

