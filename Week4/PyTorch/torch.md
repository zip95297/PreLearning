# Pytorch 的学习笔记

## PyTorch基本教学

PyTorch是一个将张量运算整理的一个python库。使用该库进行深度学习任务的代码编写。

在神经网络的训练流程中包括：准备数据集，搭建网络模型，训练，验证与测试。

### 准备数据集处理和加载

数据集会按照一定的方式将不同标签标注的数据整理成文件或者文件夹。然后在pytorch中将数据划分成batch使用迭代器输出出来。

#### DataSet

PyTorch中有抽象类**DataSet**，用于表示一个数据集并对其处理。可以通过对该类进行实现完成数据集的数据准备，包括指定数据集文件路径，指定如何读取对应标签等等。Dataset的主要作用是定义数据的抽象接口，包括数据的获取和标签的提取。它通常需要实现以下两个方法：

1. \_\_len\_\_：返回数据集的大小。
2. \_\_getitem\_\_：根据索引获取数据和标签。

作用：

- 定义数据集的抽象结构。
- 提供数据集的大小和索引访问方法。
- 支持自定义数据集加载逻辑，如从文件系统、数据库或其他来源加载数据。

#### DataLoader

而**DataLoader**是用于加载数据集的接口。它提供了批量加载数据、打乱数据顺序、多线程加载等功能。其作用如下：

- 将Dataset中的数据打包成小批量（mini-batches）。
- 支持数据的并行加载，加快数据读取速度。
- 提供数据打乱（shuffle）、数据预处理和数据增强的功能。

#### 数据处理与加载的完成流程

```python
import torch
from torch.utils.data import Dataset,DataLoader

class MyDataset(Dataset):
    def __init__(self, data, labels):
        self.data = data
        self.labels = labels
    
    def __len__(self):
        return len(self.data)
    
    def __getitem__(self, idx):
        return self.data[idx], self.labels[idx]

data = torch.randn(100, 3, 32, 32)  # 100个随机生成的图像
labels = torch.randint(0, 10, (100,))  # 100个随机生成的标签

dataset = MyDataset(data, labels)
dataloader = DataLoader(dataset, batch_size=10, shuffle=True, num_workers=4)

for batch in dataloader:
    images, labels = batch
```

其中需要注意的是DataLoader中的num_workers参数是代表在加载数据时子进程的个数，默认情况下该参数值为0，意味只用主进程加载数据，而加载数据使用子进程可以加速数据加载的速度。作用如下：

- 并行加载数据：num_workers的主要作用是通过多进程并行加载数据，从而减少I/O操作的瓶颈，提高数据加载效率。
- 提高效率：当数据加载和预处理比较耗时时，增加num_workers可以显著提高训练速度，因为模型训练和数据加载可以并行进行。

在数据集较小或I/O操作较快的情况下，设置num_workers为0，表示在主进程中加载数据，适用于简单的加载任务；对于大多数常规任务，设置num_workers为1到4通常是有效的，可以显著提高加载效率；在具有更多CPU核心的机器上，num_workers的值可以设置得更大，但需要注意系统资源的使用情况，避免过多的进程导致系统性能下降。

### Tensor高纬矩阵

在pytorch中张量tensor是一个多维的矩阵，可以通过.shape()查看张量的尺寸，其中dim0,1,2,...,i,...分别代指shape中的索引为i的纬度。比如一个张量的尺寸是(64, 10, 32, 32)如果要求将该张量索引为1的纬度的均值，其他纬度不变，则tensor.mean(dim=1)

#### Tensor的一些基础常规操作

assume `x = torch.zeros([3,2])`  
x.shape为tensor.size([3,2])

- transpose: 讲一个tensor的指定纬度调换，例如x = x.transpose(0,1)，则此时x.shape为torch.size([2,3])
- unsqueeze: 拓展出一个新的维度，例如想要使x在dim1出拓展出一个纬度，则使用x.unsqueeze(dim=1),则x.shape为torch.size([3,1,2])
- cat: 连接多个tensor，例子如下：

```python
x = torch.zeros([2,1,3]) 
y = torch.zeros([2,2,3]) 
z = torch.zeros([2,3,3]) 
w = torch.cat([x,y,z],dim=1)
# w.shape 为 torch.size([2,6,3])
```
