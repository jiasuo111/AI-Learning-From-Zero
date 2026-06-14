# Python 文件与模块操作

> 文件操作让数据可以持久化保存，`os` 和 `time` 模块则让我们能够与操作系统交互、处理时间相关的任务。

---

## 一、文件的打开与关闭

### 1.1 `open()` 函数详解

```python
open(文件路径, 打开模式, encoding='编码方式')
```

### 1.2 文件路径

| 路径类型     | 说明                   | 示例                                                         |
| ------------ | ---------------------- | ------------------------------------------------------------ |
| **绝对路径** | 以盘符开头的完整路径   | `C:/Users/data.txt`（Windows）<br>`/Users/xxx/data.txt`（Mac/Linux） |
| **相对路径** | 相对于当前程序所在目录 | `./data.txt`（当前目录）<br>`../data.txt`（上级目录）        |

### 1.3 打开模式

| 模式   | 说明           | 文件不存在 | 文件存在   |
| ------ | -------------- | ---------- | ---------- |
| `'r'`  | 只读（默认）   | 报错       | 从头读取   |
| `'w'`  | 覆盖写入       | 创建新文件 | 清空后写入 |
| `'a'`  | 追加写入       | 创建新文件 | 末尾追加   |
| `'rb'` | 二进制只读     | 报错       | 从头读取   |
| `'wb'` | 二进制覆盖写入 | 创建新文件 | 清空后写入 |
| `'ab'` | 二进制追加写入 | 创建新文件 | 末尾追加   |

### 1.4 文件编码

| 编码    | 说明                                     |
| ------- | ---------------------------------------- |
| `utf-8` | 通用编码（万国码），所有字符都有对应编码 |
| `gbk`   | 主要针对中文扩展（又称 cp936）           |

> ⚠️ **注意**：二进制模式（`rb`、`wb`、`ab`）**不需要** `encoding` 参数，否则报错！

### 1.5 基本示例

```python
# 打开文件
f = open('word.txt', mode='r', encoding='utf-8')
print(f)

# 读写文件（略）

# 关闭文件
f.close()
```

---

## 二、文件的读操作

### 2.1 读取方法

| 方法          | 说明                 |
| ------------- | -------------------- |
| `read(n)`     | 读取 n 个字符        |
| `read()`      | 读取所有剩余字符     |
| `readline()`  | 读取一行             |
| `readlines()` | 读取所有行，返回列表 |

### 2.2 示例

```python
# 打开文件
f = open('word.txt', 'r', encoding='utf-8')

# 方式1：read(n) - 读取指定个数字符
content = f.read(7)
print(content)

# 方式2：read() - 读取所有字符
content = f.read()
print(content)

# 方式3：readline() - 读取一行
content = f.readline()
print(content)

# 方式4：readlines() - 读取所有行，返回列表
lines = f.readlines()
print(lines)

# 关闭文件
f.close()
```

### 2.3 二进制读取

```python
# 读取非文本文件（图片、视频等）要用二进制模式
f = open('image.jpg', 'rb')  # 注意：不能加 encoding 参数
data = f.read()
f.close()
```

---

## 三、文件的写操作

### 3.1 写入方法

| 方法               | 说明           |
| ------------------ | -------------- |
| `write(内容)`      | 写入字符串     |
| `writelines(列表)` | 写入字符串列表 |

### 3.2 示例

```python
# 覆盖写入（w模式）
f = open('new_word.txt', 'w', encoding='utf-8')
f.write('hello world\n')
f.writelines(['你好\n', 'python\n', '文件操作\n'])
f.close()

# 追加写入（a模式）
f = open('new_word.txt', 'a', encoding='utf-8')
f.write('这是追加的一行\n')
f.close()
```

### 3.3 二进制写入

```python
# 字符串转二进制
text = "你好世界"
binary_data = text.encode('utf-8')

# 写入二进制文件
f = open('data.bin', 'wb')
f.write(binary_data)
f.close()

# 读取并解码
f = open('data.bin', 'rb')
data = f.read()
text = data.decode('utf-8')
print(text)  # 你好世界
f.close()
```

---

## 四、文件备份综合案例

### 4.1 简单备份（一次性读取）

```python
# 备份视频文件
f_in = open('sources/file.mp4', 'rb')
f_out = open('backup/file[备份].mp4', 'wb')

# 读取源文件全部内容
data = f_in.read()
# 写入备份文件
f_out.write(data)

# 关闭文件（先开后关）
f_out.close()
f_in.close()
```

### 4.2 带函数封装的备份（推荐）

```python
def file_copy(in_path):
    """
    备份文件
    :param in_path: 源文件路径
    """
    # 获取输出路径（在文件名后加[备份]）
    i = in_path.rfind('.')
    if i == -1:
        print('对不起，您输入的文件路径无效！')
        return
    
    out_path = in_path[:i] + "[备份]" + in_path[i:]
    print(f"备份文件路径：{out_path}")
    
    # 打开文件
    with open(in_path, 'rb') as f1:
        with open(out_path, 'wb') as f2:
            # 备份文件（先读再写）
            data = f1.read()
            f2.write(data)
    
    print("备份完成！")

# 调用函数
in_path = input('请您输入要备份的文件路径：')
file_copy(in_path)
```

### 4.3 大文件备份（分块读取）

> 大文件不能一次性读取到内存，需要分块处理。

```python
def file_copy_chunk(in_path, chunk_size=1024):
    """分块备份大文件"""
    i = in_path.rfind('.')
    if i == -1:
        print('文件路径无效！')
        return
    
    out_path = in_path[:i] + "[备份]" + in_path[i:]
    
    with open(in_path, 'rb') as f1:
        with open(out_path, 'wb') as f2:
            while True:
                chunk = f1.read(chunk_size)  # 每次读取 1024 字节
                if not chunk:  # 读取完毕
                    break
                f2.write(chunk)
    
    print(f"备份完成：{out_path}")
```

---

## 五、`with open()` 上下文管理器

### 5.1 为什么使用 `with open`？

传统方式需要手动关闭文件，如果中间出现异常，可能无法执行关闭代码：

```python
f = open('file.txt', 'r')
data = f.read()  # 如果这里报错，f.close() 不会执行
f.close()
```

使用 `with open` 会自动关闭文件，即使发生异常：

```python
with open('file.txt', 'r', encoding='utf-8') as f:
    data = f.read()
    print(data)
# 离开 with 代码块时自动关闭文件
```

### 5.2 同时操作多个文件

```python
with open('source.txt', 'r', encoding='utf-8') as f_read:
    with open('target.txt', 'w', encoding='utf-8') as f_write:
        f_write.write(f_read.read())

# 或者写在一行
with open('source.txt', 'r') as f1, open('target.txt', 'w') as f2:
    f2.write(f1.read())
```

---

## 六、`os` 模块

> `os` 模块是 Python 标准库的一部分，提供与操作系统交互的功能。

### 6.1 导入模块

```python
import os
```

### 6.2 文件操作

| 方法                  | 说明       | 示例                          |
| --------------------- | ---------- | ----------------------------- |
| `os.rename(old, new)` | 重命名文件 | `os.rename('a.txt', 'b.txt')` |
| `os.remove(path)`     | 删除文件   | `os.remove('a.txt')`          |

```python
import os

# 重命名文件
os.rename('sources/new_word.txt', 'sources/new_word_rename.txt')

# 删除文件
os.remove('sources/new_word_rename.txt')
```

### 6.3 目录操作

| 方法               | 说明               | 示例                       |
| ------------------ | ------------------ | -------------------------- |
| `os.mkdir(path)`   | 创建目录           | `os.mkdir('new_folder')`   |
| `os.rmdir(path)`   | 删除空目录         | `os.rmdir('empty_folder')` |
| `os.getcwd()`      | 获取当前工作目录   | `os.getcwd()`              |
| `os.chdir(path)`   | 切换工作目录       | `os.chdir('static')`       |
| `os.listdir(path)` | 获取目录下文件列表 | `os.listdir('.')`          |

```python
import os

# 获取当前工作目录
print(os.getcwd())

# 切换目录
os.chdir('static')
print(os.getcwd())

# 创建目录
os.mkdir('images')
os.mkdir('videos')

# 获取目录下所有文件和文件夹
print(os.listdir())

# 删除空目录
os.rmdir('videos')

# 注意：rmdir 只能删除空目录
# 删除非空目录需要 shutil.rmtree()
import shutil
shutil.rmtree('static')
```

---

## 七、`time` 模块

### 7.1 常用方法

| 方法               | 说明                   | 示例                           |
| ------------------ | ---------------------- | ------------------------------ |
| `time.time()`      | 获取当前时间戳（秒数） | `time.time()` → 1700000000.123 |
| `time.localtime()` | 获取本地时间结构体     | `time.localtime()`             |
| `time.strftime()`  | 格式化时间             | `time.strftime('%Y-%m-%d', t)` |

### 7.2 基本使用

```python
import time
import datetime

# 时间戳：1970年1月1日到现在的秒数
print(time.time())

# 本地时间结构体
print(time.localtime(0))   # 1970-01-01
print(time.localtime())     # 当前时间

# 格式化时间
print(time.strftime('%Y-%m-%d %H:%M:%S', time.localtime()))

# datetime 模块（更直观）
print(datetime.datetime.now())
print(datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S'))
```

### 7.3 时间格式化符号

| 符号 | 说明          | 示例 |
| ---- | ------------- | ---- |
| `%Y` | 年份（4位）   | 2024 |
| `%m` | 月份（01-12） | 06   |
| `%d` | 日期（01-31） | 08   |
| `%H` | 小时（00-23） | 14   |
| `%M` | 分钟（00-59） | 30   |
| `%S` | 秒（00-59）   | 45   |

### 7.4 计算程序运行时间

```python
import time

# 获取开始时间
start = time.time()
print(f"开始时间：{start}")

# 执行程序：累加计算 1 到 1 亿
total = 0
for i in range(1, 100000001):
    total += i
print(f"总和：{total}")

# 获取结束时间
end = time.time()
print(f"结束时间：{end}")

# 计算耗时
print(f"程序运行时间：{end - start:.2f} 秒")
```

---

## 八、知识点总结

### 文件操作核心

```python
# 打开文件
with open(路径, 模式, encoding=编码) as f:
    # 读操作
    content = f.read()
    lines = f.readlines()
    
    # 写操作
    f.write('内容')
    f.writelines(['行1\n', '行2\n'])
```

### 文件模式速查

| 需求                      | 模式   | 编码   |
| ------------------------- | ------ | ------ |
| 读文本文件                | `'r'`  | 需要   |
| 写文本文件（覆盖）        | `'w'`  | 需要   |
| 写文本文件（追加）        | `'a'`  | 需要   |
| 读二进制文件（图片/视频） | `'rb'` | 不需要 |
| 写二进制文件              | `'wb'` | 不需要 |

### os 模块常用操作

```python
import os

# 文件操作
os.rename('旧名', '新名')   # 重命名
os.remove('文件名')         # 删除文件

# 目录操作
os.mkdir('目录名')          # 创建目录
os.rmdir('目录名')          # 删除空目录
os.getcwd()                 # 获取当前目录
os.chdir('目录名')          # 切换目录
os.listdir('目录名')        # 列出目录内容
```

### time 模块常用操作

```python
import time

# 时间戳
timestamp = time.time()

# 格式化时间
time_str = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime())

# 计时
start = time.time()
# ... 执行代码 ...
print(f"耗时：{time.time() - start:.2f}秒")
```

---

## 练习题

1. **文件读写**：创建一个文件，写入 1-10 的数字（每行一个），然后读取并打印
2. **文件备份**：实现一个函数，备份任意文件（支持大文件分块读取）
3. **目录操作**：在当前目录下创建 `test` 文件夹，在里面创建 `a.txt`，然后删除
4. **计时器**：写一个程序，计算 `1+2+3+...+10000000` 的耗时

---

> 