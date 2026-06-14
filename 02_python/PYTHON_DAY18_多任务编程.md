# Python 多任务编程

> 多任务是现代操作系统的重要特性，它让计算机能够「同时」处理多个任务，大幅提升程序执行效率。本节将深入讲解进程、线程和协程三种多任务实现方式。

------

## 一、多任务概述

### 1.1 为什么需要多任务？

之前编写的程序都是**单任务**的——一个函数或方法执行完成后，另一个才能执行。要想实现多个任务同时执行，就需要使用**多任务**。

> 💡 **多任务的最大好处**：充分利用 CPU 资源，提高程序执行效率。

**举例**：使用网盘下载资料时，多个文件同时下载比一个一个下载快得多。

### 1.2 什么是多任务？

> **多任务**：在同一时间内执行多个任务。

例如，现在的电脑操作系统都是多任务操作系统，可以同时运行音乐播放器、浏览器、编辑器等多个软件。

### 1.3 多任务的两种表现形式

| 概念     | 含义                                 | 适用场景 |
| :------- | :----------------------------------- | :------- |
| **并发** | 在一段时间内**交替**执行多个任务     | 单核 CPU |
| **并行** | 在一段时间内**真正同时**执行多个任务 | 多核 CPU |

text

```
并发（单核 CPU）：
任务A → 任务B → 任务A → 任务B → ...（交替执行，速度极快，感觉像同时）

并行（多核 CPU）：
任务A ————————————→
任务B ————————————→（真正同时执行）
```



------

## 二、多进程

### 2.1 进程的概念

> **进程（Process）**：资源分配的最小单位，是操作系统进行资源分配和调度运行的基本单位。简单理解：**一个正在运行的程序就是一个进程**。

例如，正在运行的 QQ、微信、浏览器，每个都是一个独立的进程。

### 2.2 多进程的作用

**未使用多进程**：函数 A 执行完毕 → 函数 B 才能执行
**使用多进程**：函数 A 和函数 B 同时执行，效率大幅提升

### 2.3 多进程完成多任务

#### 基本语法

python

```
import multiprocessing

# 1. 创建进程对象
进程对象 = multiprocessing.Process(target=任务名, args=(), kwargs={})

# 2. 启动进程
进程对象.start()
```



#### Process 类参数说明

| 参数     | 说明                        |
| :------- | :-------------------------- |
| `target` | 执行的目标任务名（函数名）  |
| `name`   | 进程名（一般不用设置）      |
| `group`  | 进程组（目前只能使用 None） |
| `args`   | 以元组方式传递参数          |
| `kwargs` | 以字典方式传递参数          |

#### 示例：边听音乐边敲代码

python

```
import multiprocessing
import time

def music(num):
    for i in range(num):
        print('听音乐...')
        time.sleep(0.2)

def coding(count):
    for i in range(count):
        print('敲代码...')
        time.sleep(0.2)

if __name__ == '__main__':
    # 创建进程
    music_process = multiprocessing.Process(target=music, args=(3,))
    coding_process = multiprocessing.Process(target=coding, kwargs={'count': 3})
    
    # 启动进程
    music_process.start()
    coding_process.start()
```



### 2.4 获取进程编号

python

```
import os

# 获取当前进程编号
pid = os.getpid()

# 获取父进程编号
ppid = os.getppid()
```



### 2.5 进程的注意事项

#### ① 进程间不共享全局变量

python

```
import multiprocessing

my_list = []

def write_data():
    for i in range(3):
        my_list.append(i)
    print('write:', my_list)

def read_data():
    print('read:', my_list)

if __name__ == '__main__':
    write_process = multiprocessing.Process(target=write_data)
    read_process = multiprocessing.Process(target=read_data)
    
    write_process.start()
    time.sleep(1)
    read_process.start()
    # 输出：
    # write: [0, 1, 2]
    # read: []  ← 空的！进程间不共享全局变量
```



> 💡 **原因**：创建子进程时会拷贝主进程的资源，子进程是主进程的副本，操作的不是同一个进程里的全局变量。

#### ② 主进程与子进程的结束顺序

默认情况下，主进程会等待所有子进程执行完毕后才退出。

**方案一：设置守护进程**

python

```
work_process.daemon = True  # 主进程退出后，子进程立即销毁
```



**方案二：主动终止子进程**

python

```
work_process.terminate()  # 直接终止子进程
```



------

## 三、多线程

### 3.1 线程的概念

> **线程（Thread）**：程序执行的最小单位。进程只负责分配资源，真正执行程序的是线程。**一个进程中至少有一个线程**。

**形象比喻**：

- 进程 = 一个 QQ 软件
- 线程 = QQ 软件里打开的多个聊天窗口

多线程可以在一个进程内实现多任务，比多进程更节省资源。

### 3.2 多线程完成多任务

#### 基本语法

python

```
import threading

# 1. 创建线程对象
线程对象 = threading.Thread(target=任务名, args=(), kwargs={})

# 2. 启动线程
线程对象.start()
```



#### 示例

python

```
import threading
import time

def music():
    for i in range(3):
        print('听音乐...')
        time.sleep(0.2)

def coding():
    for i in range(3):
        print('敲代码...')
        time.sleep(0.2)

if __name__ == '__main__':
    music_thread = threading.Thread(target=music)
    coding_thread = threading.Thread(target=coding)
    
    music_thread.start()
    coding_thread.start()
```



### 3.3 线程执行带有参数的任务

python

```
# args 元组方式（注意：单元素元组需要逗号）
music_thread = threading.Thread(target=music, args=(3,))

# kwargs 字典方式
coding_thread = threading.Thread(target=coding, kwargs={'count': 3})
```



### 3.4 主线程和子线程的结束顺序

默认情况下，主线程会等待所有子线程执行完毕后才退出。

**设置守护线程**：

python

```
# 方式一：创建时设置
work_thread = threading.Thread(target=work, daemon=True)

# 方式二：创建后设置
work_thread.setDaemon(True)
```



### 3.5 线程间的执行顺序

多个线程的执行顺序是**无序的**，由操作系统调度决定。

python

```
import threading

def task():
    current = threading.current_thread()
    print(current)

if __name__ == '__main__':
    for i in range(5):
        sub_thread = threading.Thread(target=task)
        sub_thread.start()
```



### 3.6 线程间共享全局变量

> **重要特性**：线程间共享全局变量（因为多个线程在同一个进程中）。

python

```
import threading

my_list = []

def write_data():
    for i in range(3):
        my_list.append(i)
    print('write:', my_list)

def read_data():
    print('read:', my_list)

if __name__ == '__main__':
    write_thread = threading.Thread(target=write_data)
    read_thread = threading.Thread(target=read_data)
    
    write_thread.start()
    time.sleep(1)
    read_thread.start()
    # 输出：
    # write: [0, 1, 2]
    # read: [0, 1, 2]  ← 共享了全局变量！
```



------

## 四、进程 vs 线程对比

### 4.1 关系对比

| 对比项   | 说明                                     |
| :------- | :--------------------------------------- |
| 依附关系 | 线程依附在进程里面，没有进程就没有线程   |
| 数量关系 | 一个进程至少有一个线程，可以创建多个线程 |

### 4.2 区别对比

| 对比项       | 进程                   | 线程               |
| :----------- | :--------------------- | :----------------- |
| 全局变量     | 不共享                 | 共享               |
| 资源开销     | 大（需要独立内存空间） | 小（共享进程资源） |
| 创建速度     | 慢                     | 快                 |
| 资源分配单位 | 是                     | 否（CPU 调度单位） |
| 多核利用     | 可以                   | 受 GIL 限制        |

### 4.3 优缺点对比

| 类型     | 优点                 | 缺点                          |
| :------- | :------------------- | :---------------------------- |
| **进程** | 可以用多核，稳定性好 | 资源开销大，创建慢            |
| **线程** | 资源开销小，创建快   | 受 GIL 限制，不能充分利用多核 |

> 📌 **GIL（全局解释器锁）**：Python 的 GIL 导致同一时刻只有一个线程在执行 Python 字节码，因此多线程无法真正利用多核 CPU 进行并行计算。对于 CPU 密集型任务，多进程更适合；对于 I/O 密集型任务，多线程和协程更合适。

------

## 五、多协程

### 5.1 生成器回顾

在了解协程之前，先回顾生成器（Generator）。

#### 生成器推导式

python

```
# 列表推导式（一次性生成所有数据）
list_data = [i * 2 for i in range(5)]  # [0, 2, 4, 6, 8]

# 生成器推导式（需要时才生成）
gen_data = (i * 2 for i in range(5))   # <generator object>
print(next(gen_data))  # 0
print(next(gen_data))  # 2
```



#### yield 生成器

python

```
def my_generator(n):
    for i in range(n):
        print('开始生成...')
        yield i   # 暂停并返回 i
        print('完成一次...')

g = my_generator(3)
print(next(g))  # 开始生成... 0
print(next(g))  # 完成一次... 开始生成... 1
print(next(g))  # 完成一次... 开始生成... 2
# print(next(g))  # 第4次调用会抛出 StopIteration 异常
```



**yield 的特点**：

- 代码执行到 `yield` 会暂停，把结果返回
- 下次启动会在暂停的位置继续执行
- 生成器生成完成后，再次获取会抛出 `StopIteration` 异常
- `for` 循环会自动处理这个异常

### 5.2 协程的概念

> **协程（Coroutine）**：用户态的轻量级线程，由程序自身控制调度，而不是由操作系统调度。

Python 的协程是从生成器发展而来的，通过 `async` 和 `await` 关键字实现。

| 特性     | 生成器 (Generator)      | 协程 (Coroutine)      |
| :------- | :---------------------- | :-------------------- |
| 主要目的 | 生成值序列              | 执行异步任务          |
| 控制流   | 单向（调用者 → 生成器） | 双向（调用者 ↔ 协程） |
| 数据流向 | 向外产出值              | 双向发送和接收        |
| 调度方式 | 由调用者驱动            | 由事件循环调度        |
| 关键字   | `yield`                 | `async`、`await`      |

### 5.3 协程的基本使用

python

```
import asyncio

async def hello(name):           # 1. async 定义协程函数
    print(f"开始: {name}")
    await asyncio.sleep(1)       # 2. await 让出控制权
    print(f"结束: {name}")

async def main():
    # 方式1：单个协程调用
    await hello("Alice")
    
    # 方式2：并发执行多个协程
    task1 = asyncio.create_task(hello("Bob"))
    task2 = asyncio.create_task(hello("Charlie"))
    await task1
    await task2

# 3. 启动事件循环
asyncio.run(main())
```



**协程三要素**：

1. 函数前加 `async`
2. 等待处加 `await`
3. 启动用 `asyncio.run()`

### 5.4 协程 vs 线程 vs 进程对比

| 对比项   | 协程                    | 线程         | 进程           |
| :------- | :---------------------- | :----------- | :------------- |
| 创建数量 | 轻松上万                | 最多几百     | 最多几十       |
| 适用场景 | I/O密集型（网络、文件） | I/O密集型    | CPU密集型      |
| 内存占用 | 很小（几KB）            | 较大（几MB） | 很大（几十MB） |
| 数据共享 | 直接共享                | 需要加锁     | 不能直接共享   |
| 切换开销 | 极小                    | 较小         | 较大           |
| 利用多核 | 不能                    | 受GIL限制    | 可以           |

### 5.5 应用场景选择指南

python

```
# 选择指南
if 主要是网络请求 or 文件读写:      # I/O密集型
    用协程   # 最佳选择
    
elif 主要是数学计算:                # CPU密集型
    用多进程  # 绕过GIL
    
else:  # 简单的后台任务
    用多线程  # 简单易用
```



### 5.6 简单记忆法

| 概念     | 形象比喻                                          |
| :------- | :------------------------------------------------ |
| **协程** | 单线程魔术师，手里抛接多个球（I/O等待时切换任务） |
| **线程** | 多个魔术师，但只有一个能同时表演（GIL限制）       |
| **进程** | 多个魔术师，各自独立表演（完全独立）              |

### 5.7 执行时间对比示例

python

```
import asyncio
import time
import threading

def mock_io(delay, name):
    time.sleep(delay)
    return f"{name}完成"

# 1. 普通顺序执行（慢）
def sync_version():
    start = time.time()
    mock_io(1, "任务1")
    mock_io(1, "任务2")
    print(f"同步: {time.time()-start:.1f}秒")  # 约2秒

# 2. 多线程执行（快）
def thread_version():
    start = time.time()
    t1 = threading.Thread(target=mock_io, args=(1, "线程1"))
    t2 = threading.Thread(target=mock_io, args=(1, "线程2"))
    t1.start()
    t2.start()
    t1.join()
    t2.join()
    print(f"线程: {time.time()-start:.1f}秒")   # 约1秒

# 3. 协程执行（最快）
async def async_version():
    start = time.time()
    
    async def async_io(delay, name):
        await asyncio.sleep(delay)
        return f"{name}完成"
    
    task1 = asyncio.create_task(async_io(1, "协程1"))
    task2 = asyncio.create_task(async_io(1, "协程2"))
    await task1
    await task2
    
    print(f"协程: {time.time()-start:.1f}秒")   # 约1秒

# 注意：协程的 asyncio.sleep 是异步的，不会阻塞
# 而 time.sleep 是阻塞的，会阻塞整个线程
```



------

## 知识点总结

### 多任务实现方式速查表

| 方式   | 核心模块          | 创建方式               | 适用场景  |
| :----- | :---------------- | :--------------------- | :-------- |
| 多进程 | `multiprocessing` | `Process(target=func)` | CPU密集型 |
| 多线程 | `threading`       | `Thread(target=func)`  | I/O密集型 |
| 协程   | `asyncio`         | `async def` + `await`  | 高并发I/O |

### 进程 vs 线程 vs 协程

text

```
┌─────────────────────────────────────────────────────────────┐
│                        进程（资源单位）                        │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                    线程（调度单位）                    │    │
│  │  ┌─────────────────────────────────────────────┐    │    │
│  │  │              协程（用户态轻量级）              │    │    │
│  │  └─────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```



### 选择建议

| 场景           | 推荐方案 | 原因                    |
| :------------- | :------- | :---------------------- |
| 大量网络请求   | 协程     | 开销最小，并发最高      |
| 文件读写操作   | 多线程   | 资源开销小，实现简单    |
| 复杂计算任务   | 多进程   | 突破 GIL 限制，利用多核 |
| 简单的后台任务 | 多线程   | 简单易用                |

------

## 练习题

1. **多进程**：编写程序，创建两个进程，分别打印奇数和偶数 1-100
2. **多线程**：编写程序，创建三个线程，分别打印 1-100 的数字，观察输出顺序
3. **进程 vs 线程**：写代码验证进程间不共享全局变量，线程间共享全局变量
4. **协程**：使用 `asyncio` 编写一个程序，并发执行 5 个网络请求（使用 `asyncio.sleep` 模拟）

![04多任务](E:\黑马学习资料\各种文件\面试\01_python进阶\00总结图片\04多任务.png)