# Pytorch数据读取

[TOC]

## torch.utils.data

总结自[torch官方文档](https://pytorch.org/docs/stable/data.html#)

### DataLoader类

- 声明：torch.utils.data.DataLoader(*dataset*, *batch_size=1*, *shuffle=None*, *sampler=None*, *batch_sampler=None*, *num_workers=0*, *collate_fn=None*, *pin_memory=False*, *drop_last=False*, *timeout=0*, *worker_init_fn=None*, *multiprocessing_context=None*, *generator=None*, ***, *prefetch_factor=None*, *persistent_workers=False*, *pin_memory_device=''*)

- 参数解释：
  - dataset：torch.utils.data.dataset类型对象
  - drop_last：如果batch_size不能整除数据集长度，此时最后一个batch大小会不足一个batch_size大小，drop_last为真时将会删去这个batch。

### Dataset类

- 声明：torch.utils.data.Dataset(*args, **kwds)

### IterableDataset类

- 声明：torch.utils.data.IterableDataset(*args, **kwds)

### TensorDataset类

- 声明：torch.utils.data.TensorDataset(**tensors*)

### StackDataset类

- 声明：torch.utils.data.StackDataset(*args, **kwargs)

### ConcatDataset类

- 声明：torch.utils.data.ConcatDataset(datasets)

### ChainDataset类

- 声明：torch.utils.data.ChainDataset(datasets)

### Subset类

- 声明：torch.utils.data.Subset(dataset, indices)

### 