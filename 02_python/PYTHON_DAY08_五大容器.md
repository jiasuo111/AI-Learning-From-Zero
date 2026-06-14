# Python 容器（数据结构）完整指南

> 容器是 Python 中最重要的概念之一，它们就像「数据的收纳盒」，可以批量存储和管理数据。

---

## 一、容器概述

### 什么是容器？

> **容器**：可以容纳多个元素的 Python 数据类型。

### 五大容器分类

| 容器           | 类型   | 是否可变 | 是否有序     | 是否允许重复       | 定义符号  |
| -------------- | ------ | -------- | ------------ | ------------------ | --------- |
| **字符串 str** | 序列   | ❌ 不可变 | ✅ 有序       | ✅ 允许重复         | `''` `""` |
| **列表 list**  | 序列   | ✅ 可变   | ✅ 有序       | ✅ 允许重复         | `[]`      |
| **元组 tuple** | 序列   | ❌ 不可变 | ✅ 有序       | ✅ 允许重复         | `()`      |
| **集合 set**   | 非序列 | ✅ 可变   | ❌ 无序       | ❌ 不重复           | `{}`      |
| **字典 dict**  | 非序列 | ✅ 可变   | ✅ 有序(3.7+) | 键不重复，值可重复 | `{k:v}`   |

---

## 二、列表（List）

### 2.1 特点

> 列表是**可变**的，可以存储**任意类型**数据，**有序**且**可重复**。

### 2.2 定义方式

```python
# 空列表
l1 = []
l2 = list()

# 非空列表
l3 = ['张三', '李四', 10, 20, 3.14, True, False]

# 列表嵌套
l4 = [[1, 2], [3, 4], [5, 6]]
```

### 2.3 下标索引

```python
name_list = ['张三', '李四', '王五']

# 正索引（从左到右，从0开始）
print(name_list[0])   # 张三
print(name_list[1])   # 李四

# 负索引（从右到左，从-1开始）
print(name_list[-1])  # 王五
print(name_list[-2])  # 李四

# 二维列表（嵌套）
stu_list = [name_list, age_list]
print(stu_list[0][0])  # 第一组的第一个元素
```

### 2.4 常用操作

| 操作   | 方法           | 说明                       |
| ------ | -------------- | -------------------------- |
| **增** | `append(x)`    | 末尾添加元素               |
|        | `extend(容器)` | 依次取出容器中元素追加     |
|        | `insert(i, x)` | 在索引 i 处插入元素        |
| **删** | `pop(i)`       | 删除索引 i 处元素          |
|        | `del list[i]`  | 删除索引 i 处元素          |
|        | `remove(x)`    | 删除指定元素（只删第一个） |
|        | `clear()`      | 清空所有元素               |
|        | `del list`     | 删除整个列表               |
| **改** | `list[i] = x`  | 修改索引 i 处元素          |
| **查** | `list[i]`      | 获取索引 i 处元素          |
|        | `count(x)`     | 统计 x 出现次数            |
|        | `index(x)`     | 查找 x 的索引（第一个）    |
|        | `len(list)`    | 获取元素个数               |

```python
names = []

# 增
names.append('张三')
names.extend(['李四', '王五'])
names.insert(0, '熊大')

# 改
names[0] = '老大'

# 查
print(names[0])
print(names.count('张三'))
print(names.index('张三'))
print(len(names))

# 删
names.pop(0)
del names[0]
names.remove('张三')
names.clear()
# del names  # 删除整个列表
```

### 2.5 遍历列表

```python
names = ['熊大', '熊二', '张三', '李四']

# for 循环（推荐）
for name in names:
    print(name)

# while 循环
index = 0
while index < len(names):
    print(names[index])
    index += 1
```

### 2.6 列表推导式

```python
# 生成 1-10 的列表
nums = [i for i in range(1, 11)]

# 生成 1-10 中的偶数
evens = [i for i in range(1, 11) if i % 2 == 0]

# 复杂示例
squares = [i ** 2 for i in range(1, 6)]  # [1, 4, 9, 16, 25]
```

---

## 三、元组（Tuple）

### 3.1 特点

> 元组与列表类似，但**不可变**（创建后不能修改）。

### 3.2 定义方式

```python
# 空元组
t1 = ()
t2 = tuple()

# 非空元组
t3 = ('张三', '李四', 10, 20, 3.14)

# ⚠️ 单元素元组必须加逗号！
t4 = ('张三',)  # 正确
t5 = ('张三')   # 这是字符串，不是元组
```

### 3.3 下标索引（同列表）

```python
name_tuple = ('张三', '李四', '王五')
print(name_tuple[0])   # 张三
print(name_tuple[-1])  # 王五
```

### 3.4 常用操作

| 操作   | 方法         | 说明              |
| ------ | ------------ | ----------------- |
| **查** | `tuple[i]`   | 获取索引 i 处元素 |
|        | `count(x)`   | 统计 x 出现次数   |
|        | `index(x)`   | 查找 x 的索引     |
|        | `len(tuple)` | 获取元素个数      |

```python
names = ('张三', '李四', '王五', '张三')

print(names[0])           # 张三
print(names.count('张三')) # 2
print(names.index('张三')) # 0
print(len(names))          # 4
```

### 3.5 遍历元组

```python
names = ('熊大', '熊二', '张三', '李四')

# for 循环
for name in names:
    print(name)

# while 循环
index = 0
while index < len(names):
    print(names[index])
    index += 1
```

---

## 四、序列通用操作

> 字符串、列表、元组都属于**序列**，支持相同的操作。

### 4.1 切片

```python
# 语法：序列[开始索引:结束索引:步长]

s = 'abcde'
l = ['a', 'b', 'c', 'd', 'e']
t = ('a', 'b', 'c', 'd', 'e')

# 截取 'bd' 或 ['b', 'd']
print(s[1:4:2])   # bd
print(l[1:4:2])   # ['b', 'd']
print(t[1:4:2])   # ('b', 'd')

# 省略写法
print(s[:3])      # 从头开始，取3个 → abc
print(s[2:])      # 从索引2到最后 → cde
print(s[::-1])    # 倒序 → edcba
```

### 4.2 拼接与重复

```python
s1 = 'abc'
s2 = 'def'

# 拼接
print(s1 + s2)    # abcdef

# 重复
print(s1 * 3)     # abcabcabc
```

> ⚠️ **注意**：`+` 拼接返回新序列，`append()` 修改原列表。

---

## 五、集合（Set）

### 5.1 特点

> 集合是**可变**、**无序**、**元素唯一**的数据结构。

### 5.2 定义方式

```python
# 空集合（⚠️ {} 是空字典！）
s1 = set()

# 非空集合（自动去重）
s2 = {'张三', '李四', '张三', '王五'}  # {'张三', '李四', '王五'}

# 集合只能存储不可变类型
s3 = {10, 3.14, True, "abc", (1, 2)}  # ✅
# s4 = {[1, 2]}  # ❌ 报错！列表不能放集合里
```

### 5.3 常用操作

| 操作   | 方法                  | 说明                       |
| ------ | --------------------- | -------------------------- |
| **增** | `add(x)`              | 添加元素                   |
| **删** | `pop()`               | 随机删除一个               |
|        | `remove(x)`           | 删除指定元素（不存在报错） |
|        | `discard(x)`          | 删除指定元素（不报错）     |
|        | `clear()`             | 清空集合                   |
|        | `del set`             | 删除整个集合               |
| **改** | `difference_update()` | 修改为差集                 |
|        | `update()`            | 修改为并集                 |
| **查** | `len(set)`            | 获取元素个数               |

```python
names = set()

# 增
names.add('张三')
names.add('李四')

# 改
names.difference_update({'张三', '赵六'})  # 差集
names.update({'张三', '赵六'})             # 并集

# 查
print(len(names))

# 删
names.pop()
names.remove('张三')
names.clear()
# del names
```

### 5.4 集合运算

```python
A = {1, 2, 3, 4}
B = {3, 4, 5, 6}

print(A | B)   # 并集 {1,2,3,4,5,6}
print(A & B)   # 交集 {3,4}
print(A - B)   # 差集 {1,2}
print(A ^ B)   # 对称差 {1,2,5,6}
```

### 5.5 遍历集合

```python
# 只有 for 循环（没有索引，不支持 while）
names = {'老大', '熊二', '张三'}
for name in names:
    print(name)
```

### 5.6 集合推导式

```python
# 生成 1-10 的集合
nums = {i for i in range(1, 11)}
```

---

## 六、字典（Dictionary）

### 6.1 特点

> 字典是**键值对**（key-value）的集合，**键必须唯一且不可变**。

### 6.2 定义方式

```python
# 空字典
d1 = {}
d2 = dict()

# 非空字典
d3 = {"张三": 18, "李四": 28, "王五": 38}

# 字典嵌套
score_dict = {
    "张三": {'语文': 99, '数学': 88},
    "李四": {'语文': 89, '数学': 68}
}
```

### 6.3 常用操作

| 操作   | 方法                    | 说明                        |
| ------ | ----------------------- | --------------------------- |
| **增** | `dict[新key] = value`   | 添加新键值对                |
| **改** | `dict[旧key] = 新value` | 修改已有键的值              |
| **删** | `pop(key)`              | 删除指定键值对              |
|        | `del dict[key]`         | 删除指定键值对              |
|        | `clear()`               | 清空字典                    |
|        | `del dict`              | 删除整个字典                |
| **查** | `dict[key]`             | 获取值（key不存在报错）     |
|        | `get(key)`              | 获取值（key不存在返回None） |
|        | `keys()`                | 获取所有键                  |
|        | `values()`              | 获取所有值                  |
|        | `items()`               | 获取所有键值对              |
|        | `len(dict)`             | 获取键值对个数              |

```python
score_dict = {}

# 增
score_dict['张三'] = 99
score_dict['李四'] = 80

# 改
score_dict['张三'] = 100

# 查
print(score_dict['张三'])        # 100
print(score_dict.get('王五'))    # None
print(score_dict.keys())         # dict_keys(['张三', '李四'])
print(score_dict.values())       # dict_values([100, 80])
print(score_dict.items())        # dict_items([('张三', 100), ('李四', 80)])
print(len(score_dict))           # 2

# 删
score_dict.pop('张三')
del score_dict['李四']
score_dict.clear()
# del score_dict
```

### 6.4 遍历字典

```python
score_dict = {'李四': 80, '王五': 99, '张三': 100}

# 方式1：直接遍历（默认遍历键）
for k in score_dict:
    v = score_dict[k]
    print(f"{k}的分数是:{v}")

# 方式2：遍历 keys()
for k in score_dict.keys():
    v = score_dict.get(k)
    print(f"{k}的分数是:{v}")

# 方式3：遍历 items()（推荐）
for k, v in score_dict.items():
    print(f"{k}的分数是:{v}")
```

### 6.5 字典推导式

```python
# 生成 {1:1, 2:2, ..., 10:10}
nums = {i: i for i in range(1, 11)}
```

---

## 七、生成器推导式

```python
# 生成器表达式（返回生成器对象，不是元组）
nums = (i for i in range(1, 11))
print(nums)  # <generator object>

# 转换为元组使用
t1 = tuple(nums)
print(t1)   # (1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

---

## 八、容器类型转换

```python
l = [2, 1, 3, 1]
t = (2, 1, 3, 1)
s = '2131'
ss = {2, 1, 3}
d = {2: 'a', 1: 'a', 3: 'a'}

# 转换为列表
print(list(t))   # [2, 1, 3, 1]
print(list(s))   # ['2', '1', '3', '1']
print(list(ss))  # [1, 2, 3]
print(list(d))   # [2, 1, 3]

# 转换为元组
print(tuple(l))  # (2, 1, 3, 1)
print(tuple(s))  # ('2', '1', '3', '1')
print(tuple(ss)) # (1, 2, 3)

# 转换为集合（自动去重）
print(set(l))    # {1, 2, 3}
print(set(t))    # {1, 2, 3}
print(set(s))    # {'1', '2', '3'}

# 转换为字符串
print(str(l))    # '[2, 1, 3, 1]'
print(str(t))    # '(2, 1, 3, 1)'
```

### `eval()` 函数

```python
# eval() 可以去除字符串的引号，还原数据类型
print(eval('[2, 1, 3, 1]'))   # [2, 1, 3, 1]
print(eval('(2, 1, 3, 1)'))   # (2, 1, 3, 1)
print(eval('{1, 2, 3}'))      # {1, 2, 3}
print(eval("{2: 'a', 1: 'a'}"))  # {2: 'a', 1: 'a'}

# 基本类型
print(eval('10'))     # 10
print(eval('3.14'))   # 3.14
print(eval('True'))   # True
```

---

## 九、容器通用操作

```python
# 1. len() - 获取元素个数
len(s), len(l), len(t), len(se), len(d)

# 2. min() - 获取最小元素
min(s), min(l), min(t), min(se), min(d)

# 3. max() - 获取最大元素
max(s), max(l), max(t), max(se), max(d)

# 4. sorted() - 排序（返回列表）
sorted(s)                    # 升序
sorted(s, reverse=True)      # 降序

# 5. in / not in - 成员判断
'c' in s
'c' not in s

# 6. 拆包（解包）
a, b, c = '123'    # a='1', b='2', c='3'
a, b, c = [1, 2, 3]
a, b, c = (1, 2, 3)
a, b, c = {1, 2, 3}
a, b, c = {1: 'a', 2: 'b', 3: 'c'}  # a=1, b=2, c=3
```

---

## 知识点总结

### 五大容器速查表

| 容器   | 可变 | 有序 | 重复   | 索引 | while | 定义      |
| ------ | ---- | ---- | ------ | ---- | ----- | --------- |
| 字符串 | ❌    | ✅    | ✅      | ✅    | ✅     | `'abc'`   |
| 列表   | ✅    | ✅    | ✅      | ✅    | ✅     | `[1,2,3]` |
| 元组   | ❌    | ✅    | ✅      | ✅    | ✅     | `(1,2,3)` |
| 集合   | ✅    | ❌    | ❌      | ❌    | ❌     | `{1,2,3}` |
| 字典   | ✅    | ✅    | 键唯一 | ❌    | ❌     | `{'a':1}` |

### 推导式速查

| 容器   | 推导式语法                  |
| ------ | --------------------------- |
| 列表   | `[i for i in range(10)]`    |
| 集合   | `{i for i in range(10)}`    |
| 字典   | `{i: i for i in range(10)}` |
| 生成器 | `(i for i in range(10))`    |

---

## 练习题

1. **列表操作**：创建一个列表，包含 5 个数字，求最大值、最小值、总和、平均值
2. **去重**：用集合对 `[1,2,2,3,3,3,4,4,4,4]` 去重
3. **字典操作**：创建字典存储 3 个学生成绩，遍历输出
4. **推导式**：用一行代码生成 1-100 中所有 3 的倍数
5. **类型转换**：将字符串 `'[1,2,3]'` 转换为真正的列表

---

