# Python面向对象

[TOC]

## 类

### class的`__init__`和`__call__`区别

- 虽然形式上看`__init__`和`__call__`都是直接括号调用，但是==python的类必须先实例化才能执行`__call__`==，也就是`__call__`只能由类实例对象调用，`__init__`通过类名对象进行实例化。

### class的`__dict__`属性

- python类的`__dict__`以字典形式保存类的属性，可以通过此属性修改类变量的值

- 一种常用的方式将字典转换成类的属性的方式：

  ```python
  class Config:
      def __init__(self, entries: dict = {}):
          for k, v in entries.items():
              if isinstance(v, dict):
                  self.__dict__[k] = Config(v)
              else:
                  self.__dict__[k] = v
  ```

  