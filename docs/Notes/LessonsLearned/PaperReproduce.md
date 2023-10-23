# 论文复现经验

[TOC]

## 环境配置相关

- 很多论文中用`.sh`文件运行`.py`文件，如果要在`windows`电脑上运行，则需要借助`Git`。下面介绍在`Pycharm`中利用`Git`终端运行`.sh`文件的方法：

  - 首先需要将`Pycharm`的终端配置成`Git`的终端。操作方式为：

    > `文件 -> 设置 -> 工具 -> 终端 -> shell路径`
    >
    > 这里需要将shell路径更改为`Git`安装路径下的`bin/sh.exe`文件夹路径

  - 然后在`Pycharm`中打开终端，运行`sh xx/xx/xx.sh`。需要注意的是`Git`默认采用Python的Base环境，如果要使用其他环境，需要依次运行`source activate -> conda activate env_name`（直接运行后一句会报错）。
  - 如果源代码中使用了`DataLoader`中的`num_workers`，如果要顺利运行，需要将`num_workers`设置为0。