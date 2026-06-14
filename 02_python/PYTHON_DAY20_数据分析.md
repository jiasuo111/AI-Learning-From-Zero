# Python 数据分析三剑客

> 在 Python 数据分析领域，有三个不可或缺的核心库，被称为"数据分析三剑客"。它们共同构成了 Python 数据分析的完整生态，让数据从处理到可视化变得高效而优雅。

------

## 一、数据分析三剑客简介

| 库             | 全称                    | 定位             | 核心功能                     |
| :------------- | :---------------------- | :--------------- | :--------------------------- |
| **NumPy**      | Numerical Python        | 数值计算基础库   | 高性能多维数组对象、数学运算 |
| **Pandas**     | Panel Data              | 数据处理和分析库 | 数据清洗、整理、分析         |
| **Matplotlib** | MATLAB Plotting Library | 数据可视化库     | 丰富的图表绘制功能           |

------

## 二、NumPy（数值计算引擎）

### 2.1 什么是 NumPy？

> **NumPy（Numerical Python）** 是 Python 科学计算的基础包，提供了高性能的多维数组对象和丰富的数学函数。

**核心特点**：

| 特点            | 说明                                 |
| :-------------- | :----------------------------------- |
| 高效数组运算    | 比纯 Python 代码快 10-100 倍         |
| 丰富的数学函数  | 支持线性代数、傅里叶变换、随机数生成 |
| 广播功能        | 不同形状数组之间的数学运算           |
| 底层 C 语言实现 | 执行效率极高                         |

### 2.2 安装与导入

bash

```
# 使用 pip 安装
pip install numpy

# 使用 conda 安装
conda install numpy
```



python

```
import numpy as np  # 惯例缩写为 np
```



### 2.3 核心数据结构：ndarray

#### 创建数组

python

```
import numpy as np

# 从列表创建
arr1 = np.array([1, 2, 3, 4, 5])
print("一维数组:", arr1)

arr2 = np.array([[1, 2, 3], [4, 5, 6]])
print("二维数组:\n", arr2)

# 创建特殊数组
zeros_arr = np.zeros((3, 4))   # 全0数组
ones_arr = np.ones((2, 3))     # 全1数组
eye_arr = np.eye(3)            # 单位矩阵

# 创建序列数组
range_arr = np.arange(0, 10, 2)        # 0到10，步长为2 → [0, 2, 4, 6, 8]
linspace_arr = np.linspace(0, 1, 5)    # 0到1，等分5份 → [0, 0.25, 0.5, 0.75, 1]

# 创建随机数组
random_arr = np.random.rand(3, 3)      # 0-1均匀分布
normal_arr = np.random.randn(3, 3)     # 标准正态分布
randint_arr = np.random.randint(0, 10, (3, 3))  # 随机整数
```



#### 数组属性

python

```
arr = np.array([[1, 2, 3], [4, 5, 6]])

print("数组维度:", arr.ndim)      # 2（二维数组）
print("数组形状:", arr.shape)     # (2, 3)（2行3列）
print("数组大小:", arr.size)      # 6（元素总数）
print("数据类型:", arr.dtype)     # int64
print("每个元素字节数:", arr.itemsize)  # 8
```



### 2.4 数组操作

#### 索引和切片

python

```
arr = np.array([[1, 2, 3, 4],
                [5, 6, 7, 8],
                [9, 10, 11, 12]])

# 索引
print(arr[0, 0])      # 1（第一行第一列）
print(arr[-1])        # [9, 10, 11, 12]（最后一行）
print(arr[:, 1])      # [2, 6, 10]（所有行的第二列）

# 切片
print(arr[:2])        # 前两行
print(arr[:2, :2])    # 前两行的前两列
print(arr[::2, ::2])  # 行列都隔一个取一个

# 布尔索引
print(arr[arr > 5])   # [6, 7, 8, 9, 10, 11, 12]
```



#### 形状操作

python

```
arr = np.arange(12)

# 重塑形状
arr_2d = arr.reshape(3, 4)
print(arr_2d)

# 转置
arr_t = arr_2d.T

# 展平（flatten 返回新数组，ravel 返回视图）
arr_flat = arr_2d.flatten()
arr_flat2 = arr_2d.ravel()

# 调整大小
arr_resized = np.resize(arr, (3, 5))
```



### 2.5 数学运算

#### 基本运算

python

```
a = np.array([1, 2, 3, 4])
b = np.array([5, 6, 7, 8])

# 算术运算（逐元素）
print("加法:", a + b)        # [6, 8, 10, 12]
print("减法:", a - b)        # [-4, -4, -4, -4]
print("乘法:", a * b)        # [5, 12, 21, 32]
print("除法:", a / b)        # [0.2, 0.333, 0.428, 0.5]
print("幂运算:", a ** 2)     # [1, 4, 9, 16]

# 矩阵乘法
matrix_a = np.array([[1, 2], [3, 4]])
matrix_b = np.array([[5, 6], [7, 8]])
print(np.dot(matrix_a, matrix_b))  # 或 matrix_a @ matrix_b
```



#### 统计运算

python

```
arr = np.array([[1, 2, 3],
                [4, 5, 6],
                [7, 8, 9]])

# 全局统计
print("总和:", np.sum(arr))          # 45
print("平均值:", np.mean(arr))       # 5.0
print("标准差:", np.std(arr))        # 2.58
print("方差:", np.var(arr))          # 6.67

# 沿轴统计（axis=0 列，axis=1 行）
print("每列总和:", np.sum(arr, axis=0))   # [12, 15, 18]
print("每行总和:", np.sum(arr, axis=1))   # [6, 15, 24]

# 极值
print("最小值:", np.min(arr))         # 1
print("最大值:", np.max(arr))         # 9
print("最小值索引:", np.argmin(arr))  # 0
print("最大值索引:", np.argmax(arr))  # 8
```



### 2.6 广播机制

> **广播**：NumPy 在不同形状的数组之间进行数学运算时，会自动扩展较小数组的形状。

python

```
a = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])

b = np.array([10, 20, 30])

# b 被广播到 a 的每一行
print(a + b)
# [[11, 22, 33],
#  [14, 25, 36],
#  [17, 28, 39]]

c = np.array([[1], [2], [3]])
print(a * c)
# [[1, 2, 3],
#  [8, 10, 12],
#  [21, 24, 27]]
```



------

## 三、Pandas（数据处理工具）

### 3.1 什么是 Pandas？

> **Pandas** 是基于 NumPy 构建的库，提供了快速、灵活、易用的数据结构和数据分析工具。

**两大核心数据结构**：

| 数据结构      | 维度 | 说明                            |
| :------------ | :--- | :------------------------------ |
| **Series**    | 一维 | 带标签的数组，类似带索引的字典  |
| **DataFrame** | 二维 | 表格型数据结构，类似 Excel 表格 |

### 3.2 安装与导入

bash

```
pip install pandas
conda install pandas
```



python

```
import pandas as pd  # 惯例缩写为 pd
import numpy as np
```



### 3.3 Series（一维数据结构）

python

```
# 从列表创建
s1 = pd.Series([1, 3, 5, np.nan, 6, 8])
print(s1)
# 0    1.0
# 1    3.0
# 2    5.0
# 3    NaN
# 4    6.0
# 5    8.0
# dtype: float64

# 从字典创建
s2 = pd.Series({'a': 1, 'b': 2, 'c': 3})

# 指定索引
s3 = pd.Series([10, 20, 30], index=['x', 'y', 'z'])

# Series 属性
print("值:", s3.values)    # [10, 20, 30]
print("索引:", s3.index)   # Index(['x', 'y', 'z'], dtype='object')
```



### 3.4 DataFrame（二维表格）

#### 创建 DataFrame

python

```
# 方式1：从字典创建
data = {
    '姓名': ['张三', '李四', '王五', '赵六'],
    '年龄': [25, 30, 35, 28],
    '城市': ['北京', '上海', '广州', '深圳'],
    '工资': [5000, 7000, 6000, 8000]
}
df = pd.DataFrame(data)

# 方式2：从列表创建
data_list = [
    ['张三', 25, '北京', 5000],
    ['李四', 30, '上海', 7000],
    ['王五', 35, '广州', 6000],
    ['赵六', 28, '深圳', 8000]
]
df2 = pd.DataFrame(data_list, columns=['姓名', '年龄', '城市', '工资'], index=['s1', 's2', 's3', 's4'])

# 查看基本信息
print("形状:", df.shape)          # (4, 4)
print("列名:", df.columns)        # Index(['姓名', '年龄', '城市', '工资'], dtype='object')
print("索引:", df.index)          # RangeIndex(start=0, stop=4, step=1)
print("数据类型:\n", df.dtypes)
```



#### 数据查看

python

```
print(df.head(3))      # 前3行
print(df.tail(2))      # 后2行
print(df.info())       # 详细信息（包括缺失值）
print(df.describe())   # 描述性统计（数值列）
```



#### 数据选择

python

```
# 列选择
print(df['姓名'])           # 单列
print(df[['姓名', '年龄']]) # 多列

# 行选择（loc：标签索引）
print(df.loc[0])           # 第一行
print(df.loc[[0, 2]])      # 第0行和第2行
print(df.loc[0:2])         # 0-2行（包含结束）

# 行选择（iloc：位置索引）
print(df.iloc[0])          # 第一行
print(df.iloc[[0, 2]])     # 第0行和第2行
print(df.iloc[0:3])        # 0-2行（不包含结束）

# 条件筛选
print(df[df['年龄'] > 30])                     # 年龄大于30
print(df[(df['年龄'] > 28) & (df['工资'] < 7000)])  # 多条件
print(df.query('年龄 > 28 and 工资 < 7000'))   # query 方法

# 排序
print(df.sort_values('工资', ascending=False))          # 单列排序
print(df.sort_values(['部门', '工资'], ascending=[True, False]))  # 多列排序
```



### 3.5 数据处理和清洗

#### 处理缺失值

python

```
# 检查缺失值
print(df.isnull().sum())      # 每列缺失值数量
print(df.isnull().sum().sum()) # 总缺失值数量

# 删除缺失值
df_dropped = df.dropna()                    # 删除包含缺失值的行
df_dropped_col = df.dropna(how='all')       # 删除全为缺失值的列

# 填充缺失值
df_filled = df.fillna(0)                    # 用0填充
df_filled_mean = df.fillna(df.mean())       # 用均值填充
df_filled_ffill = df.fillna(method='ffill') # 向前填充
```



#### 分组聚合

python

```
# 单列分组
print(df.groupby('产品')['销售额'].sum())

# 多列分组
print(df.groupby(['产品', '地区'])['销售额'].agg(['sum', 'mean', 'count']))

# 数据透视表
pivot = df.pivot_table(index='产品', columns='地区', values='销售额', aggfunc='sum')
```



------

## 四、Matplotlib（可视化平台）

### 4.1 什么是 Matplotlib？

> **Matplotlib** 是 Python 最著名的绘图库，提供了一整套和 MATLAB 类似的绘图 API，适合交互式绘图。

**特点**：

| 特点         | 说明                         |
| :----------- | :--------------------------- |
| 丰富图表类型 | 线图、散点图、柱状图、饼图等 |
| 高度可定制   | 可控制图表的每一个细节       |
| 多种输出格式 | 支持 PNG、PDF、SVG 等        |
| 中文支持     | 需额外设置中文字体           |

### 4.2 安装与导入

bash

```
pip install matplotlib
conda install matplotlib
```



python

```
import matplotlib.pyplot as plt
import numpy as np

# 设置中文显示
plt.rcParams['font.sans-serif'] = ['SimHei', 'Microsoft YaHei', 'WenQuanYi Micro Hei']
plt.rcParams['axes.unicode_minus'] = False  # 解决负号显示问题

# 新版本可能需要指定后端
import matplotlib
matplotlib.use('TkAgg')
```



### 4.3 折线图

python

```
x = np.array([1, 2, 3, 4, 5])
y = np.array([2, 4, 1, 5, 3])

plt.figure(figsize=(10, 6))
plt.plot(x, y, color='blue', marker='o', linestyle='-', linewidth=2, markersize=8)

plt.title("折线图示例", fontsize=16)
plt.xlabel("X轴", fontsize=12)
plt.ylabel("Y轴", fontsize=12)
plt.grid(True, alpha=0.3)
plt.show()
```



### 4.4 散点图

python

```
np.random.seed(42)
x = np.random.randn(50)
y = x * 2 + np.random.randn(50) * 0.8

plt.figure(figsize=(10, 6))
plt.scatter(x, y, color='red', alpha=0.6, s=50)

plt.title("散点图示例", fontsize=16)
plt.xlabel("X值", fontsize=12)
plt.ylabel("Y值", fontsize=12)
plt.grid(True, alpha=0.3)
plt.show()
```



### 4.5 柱状图

python

```
categories = ['苹果', '香蕉', '橙子', '葡萄', '西瓜']
values = [25, 40, 30, 35, 45]

plt.figure(figsize=(10, 6))
plt.bar(categories, values, color='green', alpha=0.7)

plt.title("水果销量统计", fontsize=16)
plt.xlabel("水果种类", fontsize=12)
plt.ylabel("销量（万斤）", fontsize=12)
plt.grid(True, alpha=0.3, axis='y')
plt.show()
```



### 4.6 饼图

python

```
sizes = [25, 35, 20, 20]
labels = ['苹果', '香蕉', '橙子', '葡萄']
colors = ['#ff9999', '#66b3ff', '#99ff99', '#ffcc99']
explode = (0.05, 0, 0, 0)  # 突出第一块

plt.figure(figsize=(8, 8))
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct='%1.1f%%', shadow=True, startangle=90)

plt.title("水果销量占比", fontsize=16)
plt.axis('equal')  # 保证饼图是正圆
plt.show()
```



### 4.7 子图（多图布局）

python

```
# 创建 2x2 的子图布局
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# 子图1：折线图
axes[0, 0].plot(x, y, 'b-o')
axes[0, 0].set_title('折线图')
axes[0, 0].grid(True)

# 子图2：散点图
axes[0, 1].scatter(x, y, c='r', alpha=0.6)
axes[0, 1].set_title('散点图')

# 子图3：柱状图
axes[1, 0].bar(categories, values, color='g', alpha=0.7)
axes[1, 0].set_title('柱状图')
axes[1, 0].tick_params(axis='x', rotation=45)

# 子图4：饼图
axes[1, 1].pie(sizes, labels=labels, autopct='%1.1f%%')
axes[1, 1].set_title('饼图')

plt.tight_layout()
plt.show()
```



------

## 五、三者关系总结

### 5.1 层次依赖关系

text

```
┌─────────────────────────────────────────┐
│           Matplotlib（可视化层）          │
│         将数据转化为直观图表               │
├─────────────────────────────────────────┤
│            Pandas（数据处理层）           │
│       基于 NumPy，提供高级数据结构         │
├─────────────────────────────────────────┤
│             NumPy（基础层）               │
│     提供高效的多维数组和数学运算            │
└─────────────────────────────────────────┘
```



### 5.2 功能互补

| 库             | 核心功能             | 数据格式          | 优势                     |
| :------------- | :------------------- | :---------------- | :----------------------- |
| **NumPy**      | 数值计算、数学运算   | ndarray           | 计算效率高，内存占用少   |
| **Pandas**     | 数据清洗、整理、分析 | Series、DataFrame | 操作便捷，支持缺失值处理 |
| **Matplotlib** | 图表绘制、结果展示   | 图片文件          | 图表丰富，定制化程度高   |

### 5.3 完整工作流程示例

python

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# 1. NumPy：生成数据
np.random.seed(42)
data = np.random.randn(100, 3)

# 2. Pandas：整理和分析
df = pd.DataFrame(data, columns=['A', 'B', 'C'])
print(df.head())
print(df.describe())

# 3. Pandas + Matplotlib：可视化
df.cumsum().plot(figsize=(10, 6))
plt.title("累积数据趋势")
plt.xlabel("索引")
plt.ylabel("累积值")
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```



------

## 知识点总结

### NumPy 速查

python

```
# 创建数组
np.array([1,2,3])
np.zeros((3,4))
np.ones((2,3))
np.arange(0,10,2)
np.linspace(0,1,5)

# 属性
arr.shape, arr.ndim, arr.size, arr.dtype

# 操作
arr.reshape(3,4)
arr.T
arr.flatten()

# 统计
np.sum(), np.mean(), np.std(), np.min(), np.max()
```



### Pandas 速查

python

```
# 创建
pd.Series([1,2,3])
pd.DataFrame(data)

# 查看
df.head(), df.tail(), df.info(), df.describe()

# 选择
df['列名']
df.loc[行标签]
df.iloc[行位置]
df[df['列名'] > 值]

# 处理
df.dropna()
df.fillna(0)
df.groupby('列名').sum()
```



### Matplotlib 速查

python

```
# 基本图表
plt.plot(x, y)      # 折线图
plt.scatter(x, y)   # 散点图
plt.bar(x, y)       # 柱状图
plt.pie(sizes)      # 饼图

# 样式
plt.title()
plt.xlabel()
plt.ylabel()
plt.grid()
plt.legend()

# 子图
fig, axes = plt.subplots(2, 2)
```

![07数据分析三剑客](E:\黑马学习资料\各种文件\面试\01_python进阶\00总结图片\07数据分析三剑客.png)