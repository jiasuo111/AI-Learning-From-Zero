# 03_人工神经网络ANN

```
人工神经网络 ANN

> 人工神经网络是深度学习的核心，本节从神经元的结构开始，带你理解神经网络是如何工作的。


## 一、什么是神经网络模型？

**一句话定义**：仿生生物学神经元构造出的深度学习计算模型，由多个神经元组成。

> 简单说，神经网络就是模仿人脑神经元的工作方式，构建的一个数学计算模型。


## 二、神经元与神经网络

### 2.1 一个神经元
```



输入 x₁ ————┐
│
输入 x₂ ————┼——→ [加权求和] ——→ [激活函数] ——→ 输出 y
│
输入 x₃ ————┘
↑
偏置 b

text

```
**神经元的计算过程**：
1. 每个输入乘以对应的权重（w）
2. 所有加权输入求和 + 偏置（b）
3. 经过激活函数得到最终输出

> 公式：`y = 激活函数(∑(wᵢ · xᵢ) + b)`

### 2.2 多个神经元构成的网络
```



输入层 隐藏层 输出层
x₁ ─────→ ○ ────→ ○
x₂ ─────→ ○ ────→ ○ ────→ 预测结果
x₃ ─────→ ○ ────→ ○
↑ ↑
特征组合 最终输出

text

```
### 2.3 前向传播和反向传播

| 过程 | 方向 | 作用 |
|------|------|------|
| **前向传播** | 输入 → 输出 | 计算预测值 |
| **反向传播** | 输出 → 输入 | 计算梯度，更新参数 |


## 三、激活函数

### 3.1 什么是激活函数？

> 激活函数决定神经元的输出特性，核心作用是**引入非线性因素**。

> 为什么需要非线性？因为没有激活函数，多层神经网络等价于一层线性变换，无法解决复杂问题。

### 3.2 四大激活函数

| 激活函数 | 值域 | 导数范围 | 特点 | 适用场景 |
|----------|------|----------|------|----------|
| **Sigmoid** | (0, 1) | (0, 0.25] | 梯度消失严重，非0中心化 | 二分类输出层 |
| **Tanh** | (-1, 1) | (0, 1] | 0中心化，梯度消失相对缓解 | 浅隐藏层 |
| **ReLU** | [0, +∞) | 0 或 1 | 计算简单，负样本忽略 | **深隐藏层（首选）** |
| **Softmax** | (0, 1) 概率和=1 | — | 所有概率和为1 | 多分类输出层 |

### 3.3 图解对比
```



Sigmoid: S形曲线，输出在 0~1 之间
↑
1 | ──┐
| / |
| / |
0 |─┘ └──
└─────────────────→

Tanh: S形曲线，输出在 -1~1 之间
↑
1 | ──┐
| / |
| / |
-1 |─┘ └──
└─────────────────→

ReLU: 输入≥0时输出=输入，输入<0时输出=0
↑
| /
| /
| /
0 |/─────────────→
└─────────────────→

Softmax: 将多个输出转换为概率分布，总和=1

text

```
### 3.4 激活函数选择建议

| 场景 | 推荐激活函数 |
|------|-------------|
| 浅隐藏层 | Tanh |
| **深隐藏层** | **ReLU（首选）** |
| 二分类输出层 | Sigmoid |
| 多分类输出层 | Softmax |

> 💡 **ReLU 为什么是深隐藏层的首选？** 计算简单、缓解梯度消失、能让部分神经元失活从而缓解过拟合。


## 四、参数（w 和 b）初始化

### 4.1 什么是参数初始化？

> 对权重 w 和偏置 b 设定初始值的过程。

### 4.2 7 大初始化方法

| 初始化方法 | API | 特点 | 适用场景 |
|------------|-----|------|----------|
| **随机均匀** | `uniform_()` | 均匀分布随机 | 基础初始化 |
| **随机正态** | `normal_()` | 正态分布随机 | 基础初始化 |
| **全0** | `zeros_()` | 所有参数为0 | ⚠️ 会破坏对称性，不推荐 |
| **全1** | `ones_()` | 所有参数为1 | ⚠️ 会破坏对称性，不推荐 |
| **指定值** | `constant_()` | 所有参数为指定值 | ⚠️ 会破坏对称性，不推荐 |
| **He/Kaiming** | `kaiming_uniform_()` / `kaiming_normal_()` | 只考虑输入节点 | **ReLU 及其变种** |
| **Xavier** | `xavier_uniform_()` / `xavier_normal_()` | 考虑输入和输出节点 | Sigmoid、Tanh |

### 4.3 参数初始化的作用

| 作用 | 说明 |
|------|------|
| **防止梯度消失/爆炸** | 合适的初始化避免梯度在传播过程中过快衰减或增长 |
| **打破对称性** | 每个神经元权重不同，更新方向不同，学习不同的特征 |

### 4.4 激活函数 → 初始化选择

| 激活函数 | 推荐初始化 |
|----------|-----------|
| ReLU / LeakyReLU | **Kaiming（He）初始化** |
| Sigmoid / Tanh | **Xavier（Glorot）初始化** |


## 五、损失函数

### 5.1 什么是损失函数？

> 衡量模型预测值与真实值差距的函数。根据损失值，通过反向传播和梯度下降更新参数。

### 5.2 分类任务损失函数

| 损失函数 | API | 说明 | 输入要求 |
|----------|-----|------|----------|
| **多分类交叉熵** | `nn.CrossEntropyLoss()` | 多分类任务标准，底层自动 Softmax | 预测分数（logits） |
| **二分类交叉熵** | `nn.BCELoss()` | 二分类任务 | 预测概率（需要先 Sigmoid） |

> ⭐ **`nn.CrossEntropyLoss()` 底层自动执行 Softmax + 交叉熵计算**，传入预测分数即可。

### 5.3 回归任务损失函数

| 损失函数 | API | 特点 | 对异常值 |
|----------|-----|------|----------|
| **MAE（L1损失）** | `nn.L1Loss()` | 对异常值不敏感，导数恒定 | 鲁棒 |
| **MSE（L2损失）** | `nn.MSELoss()` | 对异常值敏感，梯度随误差变化 | 不鲁棒 |
| **Smooth L1** | `nn.SmoothL1Loss()` | 结合 MAE 和 MSE 的优点 | 鲁棒 |

### 5.4 图解对比
```



MAE: 误差越大，梯度恒定 → 不会跳过极小值，但收敛慢
MSE: 误差越大，梯度越大 → 收敛快，但对异常值敏感
Smooth L1: 综合两者优点，在误差小时用 MSE，误差大时用 MAE

text

```
## 六、优化器（梯度下降优化方法）

### 6.1 梯度下降公式

$$w_{new} = w_{old} - lr \times grad$$

| 符号 | 含义 |
|------|------|
| `w_old` | 当前参数 |
| `lr` | 学习率（超参数） |
| `grad` | 梯度（反向传播计算得到） |

### 6.2 优化器演进路线
```



SGD（基础）
↓ 加入动量
Momentum（动量法）
↓ 自适应学习率
Adagrad → RMSprop
↓ 动量 + 自适应学习率
Adam（深度学习首选）
↓ 解耦权重衰减
AdamW（大模型首选）

text

```
### 6.3 优化器对比

| 优化器 | 核心思路 | 优点 | 适用场景 |
|--------|----------|------|----------|
| **SGD** | 基础梯度下降 | 简单 | 小数据集 |
| **Momentum** | 指数加权平均梯度 | 跳出鞍点，震荡平滑 | 需要加速收敛 |
| **Adagrad** | 自适应学习率 | 前期学习率大，后期小 | 高维稀疏数据 |
| **RMSprop** | 指数加权平均梯度平方 | 避免 Adagrad 下降过快 | 高维稀疏数据 |
| **Adam** | Momentum + RMSprop | 收敛快，适应性强 | **深度学习首选** |
| **AdamW** | Adam + 解耦权重衰减 | 泛化更好，正则化更稳定 | **Transformer/大模型首选** |

### 6.4 学习率调度器

​```python
# 等间隔衰减：每 50 轮 × 0.5
scheduler = torch.optim.lr_scheduler.StepLR(optimizer, step_size=50, gamma=0.5)

# 多间隔衰减：在指定轮次衰减
scheduler = torch.optim.lr_scheduler.MultiStepLR(optimizer, milestones=[50, 100, 160], gamma=0.5)

# 指数衰减：每轮 × gamma
scheduler = torch.optim.lr_scheduler.ExponentialLR(optimizer, gamma=0.9)
```



## 七、正则化（缓解过拟合）

### 7.1 什么是正则化？

> 一种防止过拟合、提高模型泛化能力的策略。

### 7.2 常见正则化策略

| 策略          | 核心思想            | 效果                       |
| :------------ | :------------------ | :------------------------- |
| **L1 正则化** | 损失函数 + λ·Σ\|w\| | 权重**变为 0**，特征筛选   |
| **L2 正则化** | 损失函数 + λ·Σw²    | 权重**接近 0**，均匀缩小   |
| **Dropout**   | 随机失活神经元      | 防止过分依赖某些神经元     |
| **BatchNorm** | 规范化每层输入分布  | 加速收敛，同时有正则化效果 |

### 7.3 Dropout

python

```
# 在激活层后使用
self.dropout = nn.Dropout(p=0.4)   # 40% 神经元随机失活

# 训练/评估模式切换
model.train()   # Dropout 生效
model.eval()    # Dropout 失效
```



### 7.4 BatchNorm（批量归一化）

> 将每层输入分布规范到 均值为 0、方差为 1。

python

```
# 在卷积层/线性层后、激活层前使用
self.bn = nn.BatchNorm1d(64)   # 全连接层用
self.bn = nn.BatchNorm2d(64)   # 卷积层用

# 作用：
# 1. 加快收敛速度
# 2. 有一定的正则化效果
# 3. 缓解梯度消失
```



## 八、模型训练模板（完整版）

python

```
import torch
import torch.nn as nn
from torch.utils.data import DataLoader, TensorDataset

# 1. 定义模型
class MyModel(nn.Module):
    def __init__(self, input_dim, hidden_dim, output_dim):
        super().__init__()
        self.fc1 = nn.Linear(input_dim, hidden_dim)
        self.fc2 = nn.Linear(hidden_dim, hidden_dim)
        self.fc3 = nn.Linear(hidden_dim, output_dim)
        self.relu = nn.ReLU()
        self.dropout = nn.Dropout(0.3)
        
        # 参数初始化
        nn.init.kaiming_uniform_(self.fc1.weight)
        
    def forward(self, x):
        x = self.relu(self.fc1(x))
        x = self.dropout(x)
        x = self.relu(self.fc2(x))
        x = self.fc3(x)
        return x

# 2. 准备数据
train_loader = DataLoader(train_dataset, batch_size=32, shuffle=True)
test_loader = DataLoader(test_dataset, batch_size=32, shuffle=False)

# 3. 创建模型、损失、优化器
model = MyModel(20, 64, 4)
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=0.001)

# 4. 训练
for epoch in range(100):
    model.train()
    for x, y in train_loader:
        output = model(x)
        loss = criterion(output, y)
        
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()

# 5. 评估
model.eval()
correct = 0
total = 0
with torch.no_grad():
    for x, y in test_loader:
        output = model(x)
        pred = torch.argmax(output, dim=-1)
        correct += (pred == y).sum().item()
        total += y.size(0)

print(f"准确率: {correct/total:.4f}")
```



## 九、面试问答

### Q1：为什么需要激活函数？

> 没有激活函数，多层神经网络等价于一层线性变换，无法解决非线性问题。激活函数引入非线性，让神经网络能够拟合任意函数。

### Q2：ReLU 有什么优缺点？

> **优点**：计算简单、收敛快、缓解梯度消失。
> **缺点**：负值直接置 0，可能导致神经元「死亡」（Dead ReLU）。

### Q3：Dropout 为什么能防止过拟合？

> 每批训练随机让部分神经元失活，相当于训练了多个不同的子网络，模型不会过分依赖某些特定神经元，泛化能力更强。

### Q4：为什么 Adam 是深度学习首选优化器？

> 结合了 Momentum（加速收敛）和 RMSprop（自适应学习率），收敛快、适应性强、调参简单。

### Q5：Kaiming 初始化和 Xavier 初始化的区别？

| 对比         | Xavier              | Kaiming（He） |
| :----------- | :------------------ | :------------ |
| 适用激活函数 | Sigmoid / Tanh      | **ReLU**      |
| 考虑因素     | 输入节点 + 输出节点 | 输入节点      |

## 十、总结

### 一句话总结

> 神经网络由神经元组成，通过激活函数引入非线性，通过反向传播更新参数，通过正则化防止过拟合。

### 知识结构图

text

```
ANN
├── 神经元
│   ├── 加权求和 (wx+b)
│   └── 激活函数 (Sigmoid/Tanh/ReLU/Softmax)
├── 参数初始化
│   ├── Xavier → Sigmoid/Tanh
│   └── Kaiming → ReLU
├── 损失函数
│   ├── 分类：CrossEntropyLoss / BCELoss
│   └── 回归：MSE / MAE / SmoothL1
├── 优化器
│   ├── SGD → Momentum → Adam → AdamW
│   └── 学习率调度器
└── 正则化
    ├── L1 / L2
    ├── Dropout
    └── BatchNorm
```