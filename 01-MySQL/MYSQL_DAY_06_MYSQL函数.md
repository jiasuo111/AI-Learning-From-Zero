# MySQL 数据库基础（续）

## 第八部分：MySQL 内置函数

> 💡 **学习理念**：内置函数是已经被封装好的工具，我们不需要知道它们是如何实现的，只需要学会如何使用它们。遇到不熟悉的函数，可以查阅官方文档或询问AI。

**官方文档**：https://dev.mysql.com/doc/refman/8.0/en/functions.html

------

## 二十六、开窗函数（窗口函数）

### 1. 什么是开窗函数？

> 💡 **核心理解**：开窗函数可以在**不合并行**的情况下做聚合运算。

| 对比项   | 普通聚合函数（GROUP BY） | 开窗函数（OVER）       |
| :------- | :----------------------- | :--------------------- |
| 结果行数 | 多行合并成一行           | 保持原行数不变         |
| 典型场景 | 统计每组的总分、平均分   | 每人后面显示本组平均分 |

### 2. 基本语法

sql

```
<窗口函数> OVER([PARTITION BY 列名] [ORDER BY 列名])
```



### 3. 数据准备

sql

```
CREATE TABLE `students` (
    `ID`     INT(11)       NOT NULL AUTO_INCREMENT,
    `Name`   VARCHAR(24)   NOT NULL,
    `Gender` VARCHAR(8)    NOT NULL,
    `Score`  DECIMAL(5, 2) NOT NULL,
    PRIMARY KEY (`ID`)
);

INSERT INTO `students` VALUES
(1, 'smart', 'Male', 90.00),
(2, 'linda', 'Female', 81.00),
(3, 'lucy', 'Female', 83.00),
(4, 'david', 'Male', 94.00),
(5, 'Tom', 'Male', 92.00),
(6, 'Jack', 'Male', 88.00);
```



### 4. 基础用法：OVER()

> `OVER()` 时，每行关联的数据范围都是**整张表**的数据。

sql

```
-- 示例1：计算每个学生的分数和整体平均分的差值
SELECT 
    *,
    AVG(score) OVER() AS avg_score,
    score - AVG(score) OVER() AS diff
FROM students;

-- 示例2：计算每个学生分数占总分的百分比
SELECT 
    *,
    SUM(score) OVER() AS sum_score,
    CONCAT(ROUND(score / SUM(score) OVER() * 100, 2), '%') AS ratio
FROM students;
```



**执行结果：**

| ID   | Name  | Gender | Score | avg_score | diff  |
| :--- | :---- | :----- | :---- | :-------- | :---- |
| 1    | smart | Male   | 90.00 | 88.00     | 2.00  |
| 2    | linda | Female | 81.00 | 88.00     | -7.00 |
| 3    | lucy  | Female | 83.00 | 88.00     | -5.00 |
| 4    | david | Male   | 94.00 | 88.00     | 6.00  |
| 5    | Tom   | Male   | 92.00 | 88.00     | 4.00  |
| 6    | Jack  | Male   | 88.00 | 88.00     | 0.00  |

------

## 二十七、PARTITION BY（分区）

### 语法

sql

```
<窗口函数> OVER(PARTITION BY 列名1, 列名2, ...)
```



### 作用

- 按照指定列对数据进行**分区**
- 窗口函数作用在**每个分区**上，而不是整张表

### PARTITION BY vs GROUP BY

| 对比项   | PARTITION BY         | GROUP BY             |
| :------- | :------------------- | :------------------- |
| 使用场景 | 窗口函数中           | 分组聚合中           |
| 结果行数 | 一进一出（行数不变） | 多进一出（行数减少） |

### 示例

sql

```
-- 需求：计算每个学生的分数和同性别学生平均分的差值
SELECT 
    *,
    AVG(score) OVER(PARTITION BY gender) AS gender_avg_score,
    score - AVG(score) OVER(PARTITION BY gender) AS diff
FROM students;
```



**执行结果：**

| ID   | Name  | Gender | Score | gender_avg_score | diff  |
| :--- | :---- | :----- | :---- | :--------------- | :---- |
| 2    | linda | Female | 81.00 | 82.00            | -1.00 |
| 3    | lucy  | Female | 83.00 | 82.00            | 1.00  |
| 1    | smart | Male   | 90.00 | 91.00            | -1.00 |
| 4    | david | Male   | 94.00 | 91.00            | 3.00  |
| 5    | Tom   | Male   | 92.00 | 91.00            | 1.00  |
| 6    | Jack  | Male   | 88.00 | 91.00            | -3.00 |

------

## 二十八、排名函数（Ranking Functions）

### 语法

sql

```
<排名函数> OVER(ORDER BY 列名 [ASC|DESC])
```



### 三种排名函数对比

| 函数           | 特点             | 示例（分数：95,95,90） |
| :------------- | :--------------- | :--------------------- |
| `RANK()`       | 有并列，但不连续 | 1, 1, 3                |
| `DENSE_RANK()` | 有并列，且连续   | 1, 1, 2                |
| `ROW_NUMBER()` | 无并列，连续唯一 | 1, 2, 3                |

### 数据准备

sql

```
-- 成绩表
CREATE TABLE tb_score (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20),
    course VARCHAR(20),
    score INT
);

INSERT INTO tb_score VALUES
(1, '张三', '语文', 85),
(2, '张三', '数学', 92),
(3, '张三', '英语', 88),
(4, '李四', '语文', 78),
(5, '李四', '数学', 95),
(6, '李四', '英语', 90),
(7, '王五', '语文', 85),
(8, '王五', '数学', 88),
(9, '王五', '英语', 92),
(10, '赵六', '语文', 92),
(11, '赵六', '数学', 85),
(12, '赵六', '英语', 78);
```



### 示例

sql

```
-- 需求1：按科目对学生分数进行排名（展示三种排名方式的区别）
SELECT 
    name,
    course,
    score,
    RANK() OVER(PARTITION BY course ORDER BY score DESC) AS rk,
    DENSE_RANK() OVER(PARTITION BY course ORDER BY score DESC) AS dr,
    ROW_NUMBER() OVER(PARTITION BY course ORDER BY score DESC) AS rn
FROM tb_score;
```



**执行结果（以语文为例）：**

| name | course | score | rk   | dr   | rn   |
| :--- | :----- | :---- | :--- | :--- | :--- |
| 赵六 | 语文   | 92    | 1    | 1    | 1    |
| 张三 | 语文   | 85    | 2    | 2    | 2    |
| 王五 | 语文   | 85    | 2    | 2    | 3    |
| 李四 | 语文   | 78    | 4    | 3    | 4    |

------

## 二十九、典型应用：获取指定排名数据

### 需求：获取每个科目排名第二的学生信息

sql

```
-- 方法1：子查询方式
SELECT *
FROM (
    SELECT *,
        DENSE_RANK() OVER(PARTITION BY course ORDER BY score DESC) AS dr
    FROM tb_score
) t
WHERE dr = 2;
```



**执行结果：**

| id   | name | course | score | dr   |
| :--- | :--- | :----- | :---- | :--- |
| 1    | 张三 | 语文   | 85    | 2    |
| 3    | 张三 | 英语   | 88    | 2    |
| 8    | 王五 | 数学   | 88    | 2    |

------

## 三十、CTE（公用表表达式）

### 概念

> 💡 **CTE（Common Table Expression）**：公用表表达式，类似于子查询，相当于创建一张**临时表**，可以在 CTE 结果的基础上进行进一步的查询操作。

### 语法

sql

```
WITH 临时表名1 AS (
    -- 查询语句
), 临时表名2 AS (
    -- 查询语句
)
SELECT 字段名 FROM 临时表名;
```



### 示例

sql

```
-- 使用 CTE 获取每个科目排名第二的学生信息
WITH temp AS (
    SELECT *,
        DENSE_RANK() OVER(PARTITION BY course ORDER BY score DESC) AS dr
    FROM tb_score
)
SELECT *
FROM temp
WHERE dr = 2;
```



### CTE 的优势

| 对比项 | 子查询             | CTE                    |
| :----- | :----------------- | :--------------------- |
| 可读性 | 嵌套层级多时不易读 | 结构清晰，易读         |
| 复用性 | 不能重复使用       | 可在同一查询中多次引用 |
| 递归   | 不支持             | 支持递归查询           |

------

## 三十一、其他常用内置函数（拓展）

### 1. 数值函数

| 函数          | 说明       | 示例                | 结果     |
| :------------ | :--------- | :------------------ | :------- |
| `ROUND(x, d)` | 四舍五入   | `ROUND(3.14159, 2)` | 3.14     |
| `CEIL(x)`     | 向上取整   | `CEIL(3.14)`        | 4        |
| `FLOOR(x)`    | 向下取整   | `FLOOR(3.14)`       | 3        |
| `ABS(x)`      | 绝对值     | `ABS(-5)`           | 5        |
| `RAND()`      | 随机数 0-1 | `RAND()`            | 0.123... |

### 2. 字符串函数

| 函数                       | 说明         | 示例                        | 结果        |
| :------------------------- | :----------- | :-------------------------- | :---------- |
| `CONCAT(s1, s2)`           | 拼接字符串   | `CONCAT('Hello', ' World')` | Hello World |
| `LENGTH(s)`                | 字符串长度   | `LENGTH('MySQL')`           | 5           |
| `UPPER(s)`                 | 转大写       | `UPPER('mysql')`            | MYSQL       |
| `LOWER(s)`                 | 转小写       | `LOWER('MySQL')`            | mysql       |
| `SUBSTRING(s, start, len)` | 截取子串     | `SUBSTRING('Hello', 2, 3)`  | ell         |
| `TRIM(s)`                  | 去除首尾空格 | `TRIM(' abc ')`             | abc         |

### 3. 日期时间函数

| 函数               | 说明         | 示例                                   | 结果                |
| :----------------- | :----------- | :------------------------------------- | :------------------ |
| `NOW()`            | 当前日期时间 | `NOW()`                                | 2026-06-01 14:30:00 |
| `CURDATE()`        | 当前日期     | `CURDATE()`                            | 2026-06-01          |
| `CURTIME()`        | 当前时间     | `CURTIME()`                            | 14:30:00            |
| `YEAR(date)`       | 提取年份     | `YEAR('2026-06-01')`                   | 2026                |
| `MONTH(date)`      | 提取月份     | `MONTH('2026-06-01')`                  | 6                   |
| `DAY(date)`        | 提取天数     | `DAY('2026-06-01')`                    | 1                   |
| `DATEDIFF(d1, d2)` | 日期差值     | `DATEDIFF('2026-06-10', '2026-06-01')` | 9                   |

### 4. 流程控制函数

| 函数                     | 说明       | 示例                                                         |
| :----------------------- | :--------- | :----------------------------------------------------------- |
| `IF(条件, 真值, 假值)`   | 条件判断   | `IF(score >= 60, '及格', '不及格')`                          |
| `IFNULL(value, default)` | 空值处理   | `IFNULL(comm, 0)`                                            |
| `CASE WHEN`              | 多条件判断 | `CASE WHEN score>=90 THEN '优秀' WHEN score>=60 THEN '及格' ELSE '不及格' END` |

------

## 阶段总结

### 开窗函数核心语法

sql

```
-- 基础结构
<窗口函数> OVER(
    [PARTITION BY 分区字段]
    [ORDER BY 排序字段]
)

-- 常用组合
聚合函数 OVER()                    -- 整体聚合
聚合函数 OVER(PARTITION BY 字段)    -- 分组内聚合
排名函数 OVER(ORDER BY 字段)        -- 排序排名
排名函数 OVER(PARTITION BY 字段 ORDER BY 字段)  -- 分组内排序排名
```



### 三种排名函数的区别

| 排名函数       | 特点                | 使用场景     |
| :------------- | :------------------ | :----------- |
| `RANK()`       | 并列不连续（1,1,3） | 需要跳过名次 |
| `DENSE_RANK()` | 并列连续（1,1,2）   | 需要连续名次 |
| `ROW_NUMBER()` | 唯一连续（1,2,3）   | 需要唯一行号 |