# list

[TOC]

## 方法

### sort

- `sort(key=None, reverse=False)`方法对列表按照先从第一个维度排序，再对第一个维度相同的元素按第二个维度排序，以此类推；sort方法执行==原地排序==；sort方法默认按升序排列

- `key`参数指定每个维度用于升序排列的判断标准函数

- 示例：

  ```python
  a = [[1,2], [1,3], [0,1],[0,2]]
  a.sort()
  print(a) # [[0, 1], [0, 2], [1, 2], [1, 3]]
  a.sort(key=lambda x: (-x[0], x[1]))
  print(a) # [[1, 2], [1, 3], [0, 1], [0, 2]]
  ```

  