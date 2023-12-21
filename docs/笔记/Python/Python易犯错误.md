# Python易犯错误

[TOC]

## 列表

### 列表乘法

- 列表乘法得到的新列表的每一个元素都指向同一个地址，因此如果修改一个索引的元素会引起其他元素也相应改变。而列表表达式的各个元素则都是对应不同地址。需要注意的是如果列表表达式中元素是数字/字符（串），则每个元素也对应相同地址。==尽量用列表表达式！==

  ```python
  a = [[0]] * 2
  id(a[0]) == id(a[1]) # True
  
  a = [[0] for _ in range(2)]
  id(a[0]) == id(a[1]) # False
  
  a = ['abc' for _ in range(2)]
  id(a[0]) == id(a[1]) # True
  ```


## 内存/指针相关

### 变量内存

- python在做运算时会在内存中创建一个新的对象，因此如果要对对象做原位运算，可以使用诸如+=,[:]之类的操作。示例如下：

```python
# id(Y)改变
Y = X + Y
# id(Y)不变
Y[:] = X + Y
Y += X
```

## pandas相关

### `SettingWithCopyWarning`的解决办法

- 警告详细内容为：SettingWithCopyWarning: A value is trying to be set on a copy of a slice from a DataFrame. Try using .loc[row_indexer,col_indexer] = value instead。出现这个警告说明使用了==链式索引赋值==，因为链式索引无法确定索引得到的数据是否是原始数据的复制，因此原始数据可能并没有正确赋值。
- 解决办法：所有赋值操作都通过`.loc[index]`进行，可以先将index求出来。
