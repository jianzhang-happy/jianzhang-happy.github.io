# pip相关问题

[TOC]

## pip设置国内下载源

- 国内镜像源地址(推荐清华源)：
  - 阿里云：http://mirrors.aliyun.com/pypi/simple/
  - 豆瓣：http://pypi.douban.com/simple/
  - 清华大学：https://pypi.tuna.tsinghua.edu.cn/simple/
  - 中国科学技术大学：http://pypi.mirrors.ustc.edu.cn/simple/
  - 华中科技大学：http://pypi.hustunique.com/

- 永久设置
  - `pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`

- 临时使用国内源安装
  - `pip install -i https://pypi.tuna.tsinghua.edu.cn/simple package-name`

- 设置多个备用源

  - 更改用户目录`C:\Users\username\AppData\Roaming\pip`下的`pip`文件夹内的`pip.ini`文件

  - ```python
    [global]
    index-url = https://pypi.mirrors.ustc.edu.cn/simple/
    extra-index-url = https://pypi.mirrors.ustc.edu.cn/simple/
    		https://mirrors.aliyun.com/pypi/simple/
    		https://pypi.tuna.tsinghua.edu.cn/simple/
    		http://pypi.mirrors.ustc.edu.cn/simple/
    		https://pypi.org/simple/
    trusted-host = pypi.mirrors.ustc.edu.cn
    		pypi.mirrors.ustc.edu.cn
    		mirrors.aliyun.com
    		pypi.tuna.tsinghua.edu.cn
    		pypi.mirrors.ustc.edu.cn
    		pypi.org
    ```

  - 