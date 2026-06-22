# 02_pytorch

```
# PyTorch 深度学习框架

> PyTorch 是当前最流行的深度学习框架之一，由 Meta 开源。它灵活、易用、调试方便，是学术研究和工业应用的首选工具。


## 一、什么是 PyTorch？

**一句话定位**：PyTorch = NumPy（张量操作）+ 自动求导 + GPU加速 + 神经网络模块。

**核心功能**：
- **张量计算**（Tensor）：类似 NumPy，但支持 GPU 加速
- **自动微分**（Autograd）：自动计算梯度，无需手动推导
- **神经网络模块**（nn.Module）：封装了层、损失函数、优化器等


## 二、核心组成

| 模块 | 功能 |
|------|------|
| **torch** | 张量创建、运算、索引、形状变换 |
| **torch.autograd** | 自动微分（反向传播） |
| **torch.nn** | 神经网络层、损失函数、激活函数 |
| **torch.optim** | 优化器（SGD、Adam 等） |
| **torch.utils.data** | 数据加载（DataLoader、Dataset） |


## 三、张量（Tensor）—— 核心数据结构

### 3.1 什么是张量？

张量是多维数组，是 PyTorch 中**所有数据的存储格式**。

| 维度 | 名称 | 例子 |
|:----:|------|------|
| 0维 | 标量 | `tensor(3.14)` |
| 1维 | 向量 | `tensor([1, 2, 3])` |
| 2维 | 矩阵 | `tensor([[1,2],[3,4]])` |
| 3维+ | 张量 | 图片 `(C, H, W)` |

### 3.2 创建张量

​```python
import torch

# ----- 基本创建 -----
torch.tensor([1, 2, 3])                 # 从列表创建
torch.Tensor(2, 3)                      # 未初始化的张量

# ----- 全0/全1/指定值 -----
torch.zeros(2, 3)                       # 全0
torch.ones(2, 3)                        # 全1
torch.full((2, 3), 5)                   # 全5
torch.zeros_like(tensor)                # 形状相同，值全0

# ----- 线性序列 -----
torch.arange(0, 10, 2)                  # [0, 2, 4, 6, 8]
torch.linspace(0, 1, 5)                 # [0, 0.25, 0.5, 0.75, 1]

# ----- 随机张量 -----
torch.manual_seed(42)                   # 固定随机种子
torch.rand(2, 3)                        # 均匀分布 [0, 1)
torch.randn(2, 3)                       # 标准正态分布
torch.randint(0, 10, (2, 3))            # 随机整数
```



### 3.3 张量运算

python

```
# ----- 基础运算（逐元素）-----
t1 + t2
torch.add(t1, t2)
t1.add_(t2)                          # 下划线 = 原地修改

# ----- 矩阵乘法（面试重点⭐）-----
# 规则：(n, m) × (m, p) = (n, p)
t1 @ t2
torch.matmul(t1, t2)

# ----- 统计运算 -----
torch.mean(tensor, dim=0)            # 沿指定维度求平均
torch.sum(tensor, dim=1)             # 沿指定维度求和
torch.max(tensor, dim=0)             # 返回最大值和索引

# ----- 形状操作 -----
tensor.shape / tensor.size()         # 获取形状
tensor.reshape(2, -1)                # 改变形状（-1 自动计算）
tensor.view(2, -1)                   # 类似 reshape，要求内存连续
tensor.unsqueeze(0)                  # 升维（在第0维增加维度1）
tensor.squeeze()                     # 降维（删除所有维度为1的维度）
tensor.transpose(0, 1)               # 交换两个维度
tensor.permute(2, 0, 1)              # 重排维度顺序
```



### 3.4 张量索引

python

```
# ----- 单独获取 -----
tensor[0, :]              # 第0行
tensor[:, 1]              # 第1列
tensor[0, 1]              # 第0行第1列

# ----- 列表索引 -----
tensor[[0, 2], :]         # 第0行和第2行
tensor[:, [0, 2]]         # 第0列和第2列
tensor[[0, 2], [0, 2]]    # (0,0) 和 (2,2) 两个点

# ----- 切片索引 -----
tensor[0:2, 1:3]          # 前2行，第1-2列

# ----- 布尔索引 -----
tensor[tensor > 0.5]      # 获取所有大于0.5的元素

# ----- 多维索引 -----
tensor[0, 1, 2]           # 3维张量，第0轴索引1，第1轴索引2，第2轴索引3
```



### 3.5 张量类型转换

python

```
# 三种方式（推荐第一种）
tensor.float()                    # 推荐
tensor.type(torch.float32)
tensor.to(torch.float32)

# 常用类型
tensor.long() / tensor.int()      # 整数
tensor.float() / tensor.double()  # 浮点数
tensor.cuda() / tensor.cpu()      # 设备转换
```



### 3.6 Tensor vs NumPy

| 对比     | NumPy              | PyTorch Tensor                 |
| :------- | :----------------- | :----------------------------- |
| GPU加速  | ❌ 不支持           | ✅ 支持（`.cuda()`）            |
| 自动求导 | ❌ 不支持           | ✅ 支持（`requires_grad=True`） |
| 适用场景 | 数据预处理、后处理 | 模型训练、推理                 |

**互转**：

python

```
# Tensor → NumPy
tensor.numpy()                     # 共享内存
tensor.numpy().copy()              # 不共享内存

# NumPy → Tensor
torch.from_numpy(ndarray)          # 共享内存
torch.tensor(ndarray)              # 不共享内存

# 标量 ↔ Tensor
torch.tensor(3.14)                 # 标量 → Tensor
tensor.item()                      # 单元素 Tensor → 标量
```



## 四、自动微分（Autograd）

### 4.1 什么是自动微分？

> 自动计算损失函数对参数的梯度，用于反向传播更新参数。

**核心流程**：

text

```
开启 requires_grad=True → 前向传播计算 loss → loss.backward() 自动求导 → 用梯度更新参数
```



### 4.2 核心用法

python

```
# 1. 创建需要求导的张量
w = torch.tensor([1.0], requires_grad=True)

# 2. 前向传播
loss = (w * 2 - 4) ** 2

# 3. 反向传播（计算梯度）
loss.backward()

# 4. 查看梯度
print(w.grad)                    # tensor([-8.])

# 5. 更新参数（必须在 no_grad 下！）
with torch.no_grad():
    w -= 0.1 * w.grad
    w.grad.zero_()               # ⭐ 梯度清零（重要！否则会累加）
```



### 4.3 关键点

| 关键点                 | 说明                                              |
| :--------------------- | :------------------------------------------------ |
| `requires_grad=True`   | 标记需要求导的张量                                |
| `loss.backward()`      | 计算梯度，存到 `.grad` 属性                       |
| 梯度累加               | 每次 `backward()` 梯度会累加，必须手动 `.zero_()` |
| `with torch.no_grad()` | 更新参数时关闭自动求导，避免构建计算图            |

## 五、神经网络模块（nn.Module）

### 5.1 构建模型的模板

python

```
import torch.nn as nn

class MyModel(nn.Module):
    def __init__(self):
        super().__init__()
        # 定义网络结构
        self.fc1 = nn.Linear(20, 64)       # 全连接层
        self.fc2 = nn.Linear(64, 32)
        self.fc3 = nn.Linear(32, 4)        # 输出层（4分类）
        self.relu = nn.ReLU()

    def forward(self, x):
        # 前向传播
        x = self.relu(self.fc1(x))
        x = self.relu(self.fc2(x))
        x = self.fc3(x)                    # 输出层不加激活（CrossEntropyLoss自带softmax）
        return x
```



### 5.2 常用层

| 类别       | API                                                    | 说明               |
| :--------- | :----------------------------------------------------- | :----------------- |
| 全连接     | `nn.Linear(in, out)`                                   | 输入 × 权重 + 偏置 |
| 卷积       | `nn.Conv2d(in, out, kernel)`                           | 图像特征提取       |
| 池化       | `nn.MaxPool2d(kernel)`                                 | 降维、增大感受野   |
| 激活函数   | `nn.ReLU()` / `nn.Sigmoid()` / `nn.Tanh()`             | 引入非线性         |
| 损失函数   | `nn.CrossEntropyLoss()`（分类） `nn.MSELoss()`（回归） | 计算误差           |
| 正则化     | `nn.Dropout(p)`                                        | 随机失活防过拟合   |
| 批量归一化 | `nn.BatchNorm1d()` / `nn.BatchNorm2d()`                | 加速收敛、稳定训练 |

## 六、训练完整流程

### 6.1 训练代码模板

python

```
# 1. 准备数据
train_loader = DataLoader(dataset, batch_size=32, shuffle=True)

# 2. 创建模型、损失函数、优化器
model = MyModel()
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=0.001)

# 3. 训练循环
for epoch in range(epochs):
    for x, y in train_loader:
        # 前向传播
        output = model(x)
        loss = criterion(output, y)

        # 反向传播
        optimizer.zero_grad()      # ⭐ 梯度清零
        loss.backward()            # 计算梯度
        optimizer.step()           # 更新参数
```



### 6.2 train() vs eval()

python

```
model.train()          # 训练模式：Dropout、BN 生效
model.eval()           # 评估模式：Dropout 失效，BN 用全局统计

# ⭐ 评估时使用 with torch.no_grad() 加速
with torch.no_grad():
    output = model(x)
```



## 七、优化器

### 7.1 常用优化器

python

```
optimizer = torch.optim.SGD(model.parameters(), lr=0.01, momentum=0.9)
optimizer = torch.optim.Adam(model.parameters(), lr=0.001)
optimizer = torch.optim.RMSprop(model.parameters(), lr=0.001)
```



### 7.2 学习率调度器

python

```
# 每 50 轮将学习率乘以 0.5
scheduler = torch.optim.lr_scheduler.StepLR(optimizer, step_size=50, gamma=0.5)

# 每轮结束后调用
for epoch in range(epochs):
    train(...)
    scheduler.step()
```



## 八、核心流程总结

### 8.1 神经网络训练流程

text

```
神经元对输入做 wx+b 线性加权 → 激活函数（如 ReLU）→ 非线性输出
                    ↓
              多层堆叠 → 前向传播得到预测值
                    ↓
         预测值和真实值计算损失（交叉熵 / MSE）
                    ↓
         反向传播计算损失对每个 w / b 的梯度
                    ↓
       优化器（SGD/Adam）用梯度更新参数，减小损失
```



### 8.2 一句话总结

> **PyTorch 的核心就是：Tensor 存数据 → autograd 算梯度 → nn 搭模型 → optim 更新参数。**

## 九、面试回答模板（总分总）

> **总（15秒）**：PyTorch 是深度学习的核心框架，核心功能是张量计算和自动微分，用于构建和训练神经网络。

> **分（90秒）**：
>
> - **张量（Tensor）**：是多维数组，支持 GPU 加速和自动求导，与 NumPy 互转方便。
> - **自动微分（autograd）**：用 `requires_grad=True` 标记，`loss.backward()` 自动计算梯度。关键点：梯度会累加，每轮要 `.zero_()` 清零；参数更新要在 `with torch.no_grad():` 下进行。
> - **模型构建（nn.Module）**：继承 `nn.Module`，在 `__init__` 里定义层，在 `forward` 里写前向传播。
> - **优化器（optim）**：`torch.optim` 提供 SGD、Adam 等算法，用 `optimizer.step()` 更新参数。

> **总（10秒）**：PyTorch 用 Tensor 存数据 → autograd 算梯度 → nn 搭模型 → optim 更新参数，四步完成深度学习训练。