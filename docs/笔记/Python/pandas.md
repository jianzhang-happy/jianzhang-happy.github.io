# Pandas

[TOC]

## 常用函数/方法

### `pd.melt`

- 功能：对原始`dataframe`进行逆透视（`unpivot`）
- 用法：`pandas.melt(frame, id_vars=None, value_vars=None, var_name=None, value_name='value', col_level=None, ignore_index=True)`
- 参数：

### df.eval或pd.eval

- 功能：对dataframe的列执行一些算数操作，例如两列之间的加减乘除等

- 调用：`DataFrame.eval(expr, *, inplace=False, **kwargs)`

- 示例：

  ```python
  df = pd.DataFrame({'A': range(1, 6), 'B': range(10, 0, -2)})
  df.eval('A + B')
  ```

  
