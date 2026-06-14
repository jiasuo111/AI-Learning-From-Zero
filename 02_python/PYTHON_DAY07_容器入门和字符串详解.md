# Python 数据结构（容器）

> 「容器」——用来**批量存储数据**的结构。

---

## 一、为什么需要数据结构？

### 问题场景

```python
# 如果只有变量，存储多个数据很麻烦
name1 = "张三"
name2 = "李四"
name3 = "王五"
name4 = "赵六"
# ... 如果有100个人呢？
```

### 解决方案

使用**容器**（数据结构）来批量存储数据：

```python
names = ["张三", "李四", "王五", "赵六"]  # 一个变量存储所有名字
```

---

## 二、Python 四大数据结构对比

| 数据结构       | 是否有序              | 是否可变 | 是否允许重复     | 定义符号       |
| -------------- | --------------------- | -------- | ---------------- | -------------- |
| **列表 list**  | ✅ 有序                | ✅ 可变   | ✅ 允许重复       | `[]`           |
| **元组 tuple** | ✅ 有序                | ❌ 不可变 | ✅ 允许重复       | `()`           |
| **集合 set**   | ❌ 无序                | ✅ 可变   | ❌ 不允许重复     | `{}`           |
| **字典 dict**  | ✅ 有序（Python 3.7+） | ✅ 可变   | 键唯一，值可重复 | `{key: value}` |

---

## 三、列表（List）

### 3.1 特点

> **列表**是 Python 中最常用的数据结构，可以存储**任意类型**的数据，**有序**且**可变**。

### 3.2 定义方式

```python
# 空列表
list1 = []
list2 = list()

# 带初始值
fruits = ["苹果", "香蕉", "橙子"]
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]  # 可以混搭
```

### 3.3 访问元素（索引）

```python
fruits = ["苹果", "香蕉", "橙子", "葡萄"]

# 正向索引（从0开始）
print(fruits[0])  # 苹果
print(fruits[1])  # 香蕉
print(fruits[2])  # 橙子

# 反向索引（从-1开始）
print(fruits[-1])  # 葡萄（最后一个）
print(fruits[-2])  # 橙子
```

### 3.4 切片操作

```python
fruits = ["苹果", "香蕉", "橙子", "葡萄", "西瓜"]

# 语法：[起始:结束:步长]
print(fruits[0:3])   # ['苹果', '香蕉', '橙子']（结束索引不包含）
print(fruits[1:4])   # ['香蕉', '橙子', '葡萄']
print(fruits[:3])    # 省略起始，从头开始 → ['苹果', '香蕉', '橙子']
print(fruits[2:])    # 省略结束，到最后 → ['橙子', '葡萄', '西瓜']
print(fruits[::2])   # 步长为2 → ['苹果', '橙子', '西瓜']
print(fruits[::-1])  # 倒序 → ['西瓜', '葡萄', '橙子', '香蕉', '苹果']
```

### 3.5 常用方法

| 方法           | 说明                          | 示例                              |
| -------------- | ----------------------------- | --------------------------------- |
| `append(x)`    | 末尾添加一个元素              | `fruits.append("芒果")`           |
| `insert(i, x)` | 在索引 i 处插入元素           | `fruits.insert(1, "草莓")`        |
| `extend(iter)` | 扩展列表（合并）              | `fruits.extend(["榴莲", "山竹"])` |
| `remove(x)`    | 删除第一个匹配的元素          | `fruits.remove("香蕉")`           |
| `pop(i)`       | 删除索引 i 处元素（默认末尾） | `fruits.pop()`                    |
| `index(x)`     | 查找元素的索引                | `fruits.index("橙子")`            |
| `sort()`       | 排序（原地修改）              | `numbers.sort()`                  |
| `reverse()`    | 反转（原地修改）              | `fruits.reverse()`                |
| `len()`        | 获取长度（内置函数）          | `len(fruits)`                     |

```python
# 示例代码
fruits = ["苹果", "香蕉", "橙子"]

# 添加
fruits.append("葡萄")      # ['苹果', '香蕉', '橙子', '葡萄']
fruits.insert(1, "草莓")   # ['苹果', '草莓', '香蕉', '橙子', '葡萄']

# 删除
fruits.remove("香蕉")      # ['苹果', '草莓', '橙子', '葡萄']
last = fruits.pop()        # 删除并返回最后一个元素，last = '葡萄'

# 查询
print(fruits.index("橙子")) # 2
print(len(fruits))          # 3

# 排序
numbers = [3, 1, 4, 1, 5, 9, 2]
numbers.sort()              # [1, 1, 2, 3, 4, 5, 9]
numbers.sort(reverse=True)  # [9, 5, 4, 3, 2, 1, 1]
```

### 3.6 遍历列表

```python
fruits = ["苹果", "香蕉", "橙子"]

# 方式1：直接遍历元素
for fruit in fruits:
    print(fruit)

# 方式2：遍历索引
for i in range(len(fruits)):
    print(f"{i}: {fruits[i]}")

# 方式3：同时获取索引和元素（推荐）
for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")
```

---

## 四、元组（Tuple）

### 4.1 特点

> **元组**与列表类似，但**不可变**（创建后不能修改）。通常用于存储**不应改变的数据**。

### 4.2 定义方式

```python
# 空元组
tuple1 = ()
tuple2 = tuple()

# 带初始值
colors = ("红", "绿", "蓝")
single = (1,)  # 注意：单元素元组需要逗号！

# 不加逗号会被当作普通括号
not_tuple = (1)   # 这是整数 1，不是元组
```

### 4.3 访问元素（同列表）

```python
colors = ("红", "绿", "蓝")
print(colors[0])   # 红
print(colors[-1])  # 蓝
print(colors[0:2]) # ('红', '绿')
```

### 4.4 不可变性演示

```python
colors = ("红", "绿", "蓝")
# colors[0] = "黄"  # 报错！元组不支持修改

# 可以重新赋值（改变指向）
colors = ("黄", "绿", "蓝")  # 这是可以的
```

### 4.5 元组的使用场景

```python
# 1. 函数返回多个值
def get_user():
    return "张三", 18, "北京"  # 实际返回的是元组

name, age, city = get_user()  # 元组解包

# 2. 作为字典的键（列表不行）
locations = {("北京", "朝阳"): "798艺术区"}

# 3. 保护数据不被意外修改
WEEKDAYS = ("一", "二", "三", "四", "五", "六", "日")
```

---

## 五、集合（Set）

### 5.1 特点

> **集合**是无序的、**元素唯一**的数据结构，类似于数学中的集合。

### 5.2 定义方式

```python
# 空集合（注意：{} 是空字典！）
set1 = set()

# 带初始值
fruits = {"苹果", "香蕉", "橙子", "苹果"}  # 自动去重 → {'苹果', '香蕉', '橙子'}

# 从列表去重
numbers = set([1, 2, 2, 3, 3, 3])  # {1, 2, 3}
```

### 5.3 常用方法

| 方法         | 说明                     | 示例                     |
| ------------ | ------------------------ | ------------------------ |
| `add(x)`     | 添加元素                 | `fruits.add("葡萄")`     |
| `remove(x)`  | 删除元素（不存在报错）   | `fruits.remove("香蕉")`  |
| `discard(x)` | 删除元素（不存在不报错） | `fruits.discard("榴莲")` |
| `pop()`      | 随机删除并返回一个元素   | `fruits.pop()`           |
| `clear()`    | 清空集合                 | `fruits.clear()`         |

### 5.4 集合运算

```python
A = {1, 2, 3, 4}
B = {3, 4, 5, 6}

# 并集
print(A | B)  # {1, 2, 3, 4, 5, 6}
print(A.union(B))

# 交集
print(A & B)  # {3, 4}
print(A.intersection(B))

# 差集（A有但B没有）
print(A - B)  # {1, 2}
print(A.difference(B))

# 对称差（不同时在A和B中的）
print(A ^ B)  # {1, 2, 5, 6}
print(A.symmetric_difference(B))
```

---

## 六、字典（Dictionary）

### 6.1 特点

> **字典**是**键值对**（key-value）的集合，通过键来访问值，**键必须唯一且不可变**。

### 6.2 定义方式

```python
# 空字典
dict1 = {}
dict2 = dict()

# 带初始值
person = {
    "name": "张三",
    "age": 18,
    "city": "北京"
}

# 使用 dict() 函数
person2 = dict(name="李四", age=20, city="上海")
```

### 6.3 访问和修改

```python
person = {"name": "张三", "age": 18}

# 访问（如果键不存在会报错）
print(person["name"])    # 张三
print(person.get("age")) # 18
print(person.get("sex", "未知"))  # 不存在时返回默认值 → 未知

# 修改/添加
person["age"] = 19        # 修改已存在的键
person["sex"] = "男"      # 添加新键值对

# 删除
del person["age"]         # 删除键值对
person.pop("city")        # 删除并返回值
person.popitem()          # 删除最后一个键值对
```

### 6.4 常用方法

| 方法                | 说明           | 示例                         |
| ------------------- | -------------- | ---------------------------- |
| `keys()`            | 获取所有键     | `person.keys()`              |
| `values()`          | 获取所有值     | `person.values()`            |
| `items()`           | 获取所有键值对 | `person.items()`             |
| `get(key, default)` | 安全获取值     | `person.get("age", 0)`       |
| `update(dict)`      | 合并字典       | `person.update({"age": 19})` |

### 6.5 遍历字典

```python
person = {"name": "张三", "age": 18, "city": "北京"}

# 遍历键
for key in person.keys():
    print(key, person[key])

# 遍历值
for value in person.values():
    print(value)

# 同时遍历键和值（推荐）
for key, value in person.items():
    print(f"{key}: {value}")
```

---

## 七、列表推导式

> 列表推导式是一种**简洁创建列表**的方法。

### 基本语法

```python
[表达式 for 变量 in 可迭代对象 if 条件]
```

### 示例

```python
# 传统方式
squares = []
for i in range(1, 11):
    squares.append(i ** 2)
# [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

# 列表推导式
squares = [i ** 2 for i in range(1, 11)]

# 带条件：只取偶数
evens = [i for i in range(1, 21) if i % 2 == 0]
# [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]

# 条件表达式
results = ["偶数" if i % 2 == 0 else "奇数" for i in range(1, 6)]
# ['奇数', '偶数', '奇数', '偶数', '奇数']
```

---

## 八、深浅拷贝（了解）

### 8.1 赋值 vs 拷贝

```python
# 赋值：只是引用，修改一个会影响另一个
a = [1, 2, 3]
b = a
b[0] = 100
print(a)  # [100, 2, 3] 也被修改了！

# 浅拷贝：创建新对象，但内层对象仍是引用
import copy
a = [1, 2, [3, 4]]
b = copy.copy(a)
b[2][0] = 100
print(a)  # [1, 2, [100, 4]] 内层还是被修改了！

# 深拷贝：完全独立
a = [1, 2, [3, 4]]
b = copy.deepcopy(a)
b[2][0] = 100
print(a)  # [1, 2, [3, 4]] 完全独立
```

---

## 知识点总结

### 数据结构速查表

| 操作 | 列表 list | 元组 tuple | 集合 set  | 字典 dict  |
| ---- | --------- | ---------- | --------- | ---------- |
| 定义 | `[1,2,3]` | `(1,2,3)`  | `{1,2,3}` | `{'a':1}`  |
| 有序 | ✅         | ✅          | ❌         | ✅          |
| 可变 | ✅         | ❌          | ✅         | ✅          |
| 重复 | ✅         | ✅          | ❌         | 键不可重复 |
| 索引 | `[0]`     | `[0]`      | ❌         | `['key']`  |

---

## 练习题

1. **列表操作**：创建一个包含 10 个数字的列表，找出其中的最大值、最小值、总和
2. **去重**：给定 `[1, 2, 2, 3, 3, 3, 4, 4, 4, 4]`，用集合去重
3. **字典操作**：创建一个学生字典（姓名、年龄、成绩），遍历输出所有信息
4. **列表推导式**：用一行代码生成 1-100 中所有 3 的倍数
