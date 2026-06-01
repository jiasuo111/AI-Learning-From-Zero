# 单表查询（核心）

> 💡 查询是 SQL 中使用最频繁的操作，请重点掌握！

## 九、数据准备

sql

```
CREATE DATABASE day02_db;
USE day02_db;

CREATE TABLE IF NOT EXISTS products
(
    id          INT PRIMARY KEY AUTO_INCREMENT,
    name        VARCHAR(24)    NOT NULL,
    price       DECIMAL(10, 2) NOT NULL,
    score       DECIMAL(5, 2),
    is_self     VARCHAR(8),
    category_id INT
);

INSERT INTO products VALUES
(1, '华为Mate50', 5499.00, 9.70, '自营', 1),
(2, '荣耀80', 2399.00, 9.50, '自营', 1),
(3, '荣耀80', 2199.00, 9.30, '非自营', 1),
(4, '红米note 11', 999.00, 9.00, '非自营', 1),
(5, '联想小新14', 4199.00, 9.20, '自营', 2),
(6, '惠普战66', 4499.90, 9.30, '自营', 2),
(7, '苹果Air13', 6198.00, 9.10, '非自营', 2),
(8, '华为MateBook14', 5599.00, 9.30, '非自营', 2),
(9, '兰蔻小黑瓶', 1100.00, 9.60, '自营', 3),
(10, '雅诗兰黛粉底液', 920.00, 9.40, '自营', 3),
(11, '阿玛尼红管405', 350.00, NULL, '非自营', 3),
(12, '迪奥996', 330.00, 9.70, '非自营', 3);
```



------

## 十、基础查询

### 基本格式

sql

```
SELECT 字段名 FROM 表名;
```



### 练习示例

sql

```
-- 1. 查看所有商品
SELECT * FROM products;

-- 2. 查看所有商品的名称和价格
SELECT name, price FROM products;

-- 3. 起别名（AS 可以省略）
SELECT name AS 商品名称, price AS 价格 FROM products;
SELECT name 商品名称, price 价格 FROM products;

-- 4. 给表起别名
SELECT p.name, p.price FROM products AS p;

-- 5. 去重查看分类编号
SELECT DISTINCT category_id FROM products;
```



------

## 十一、条件查询（WHERE）

### 基本格式

sql

```
SELECT 字段名 FROM 表名 WHERE 条件;
```



### 运算符说明

| 类型 | 运算符                             |
| :--- | :--------------------------------- |
| 比较 | `>` `<` `>=` `<=` `=` `!=` `<>`    |
| 逻辑 | `AND` `OR` `NOT`                   |
| 范围 | `BETWEEN a AND b`、`IN (值1, 值2)` |
| 模糊 | `LIKE`（`%`任意多个，`_`一个字符） |
| 空值 | `IS NULL`、`IS NOT NULL`           |

### 练习示例

sql

```
-- 比较运算符
SELECT * FROM products WHERE is_self = '自营';
SELECT * FROM products WHERE score > 9.50;
SELECT * FROM products WHERE price < 999;
SELECT * FROM products WHERE score != 9.30;
SELECT * FROM products WHERE score <> 9.30;  -- 另一种写法

-- 逻辑与范围
SELECT * FROM products WHERE is_self = '自营' AND price > 2000;
SELECT * FROM products WHERE score BETWEEN 9.0 AND 9.5;
SELECT * FROM products WHERE price IN (999, 2199, 2399);
SELECT * FROM products WHERE name IN ('华为Mate50', '荣耀80');

-- 模糊查询
SELECT * FROM products WHERE name LIKE '华%';      -- 以"华"开头
SELECT * FROM products WHERE name LIKE '%66';      -- 以"66"结尾
SELECT * FROM products WHERE name LIKE '%兰%';     -- 包含"兰"
SELECT * FROM products WHERE name LIKE '__兰%';    -- 第3个字是"兰"

-- 空值判断
SELECT * FROM products WHERE score IS NULL;
SELECT * FROM products WHERE score IS NOT NULL;
```

## 十二、排序查询（ORDER BY）

### 基本格式

sql

```
SELECT 字段名 FROM 表名 ORDER BY 字段名1 [ASC|DESC], 字段名2 [ASC|DESC];
```



| 关键字 | 说明                 |
| :----- | :------------------- |
| `ASC`  | 升序（默认，可不写） |
| `DESC` | 降序                 |

> 💡 如果 `ORDER BY` 后跟多个字段，先按第一个字段排序，第一个字段相同时再按第二个字段排序。

### 练习示例

sql

```
-- 示例1：按评分从高到低排序
SELECT * FROM products ORDER BY score DESC;

-- 示例2：先按评分降序，评分相同的再按价格升序
SELECT * FROM products ORDER BY score DESC, price;
```



------

## 十三、聚合函数

### 常用聚合函数

| 函数          | 作用     | 特点         |
| :------------ | :------- | :----------- |
| `COUNT(字段)` | 统计数量 | 忽略 NULL 值 |
| `COUNT(*)`    | 统计行数 | 不忽略 NULL  |
| `SUM(字段)`   | 求和     | 忽略 NULL    |
| `AVG(字段)`   | 平均值   | 忽略 NULL    |
| `MAX(字段)`   | 最大值   | 忽略 NULL    |
| `MIN(字段)`   | 最小值   | 忽略 NULL    |

### 练习示例

sql

```
-- 示例1：统计商品总数
SELECT COUNT(id) FROM products;   -- 结果：12
SELECT COUNT(*) FROM products;    -- 结果：12（推荐）

-- 示例2：对评分列进行统计
SELECT
    COUNT(score) AS 评分个数,      -- 结果：11（有一个NULL被忽略）
    MAX(score) AS 最高分,
    MIN(score) AS 最低分,
    SUM(score) AS 总分,
    AVG(score) AS 平均分
FROM products;

-- 示例3：统计非自营商品的平均评分
SELECT AVG(score) 
FROM products 
WHERE is_self = '非自营';

-- 示例4：统计有评分的商品个数
SELECT COUNT(score) FROM products;  -- 方法1
SELECT COUNT(*) FROM products WHERE score IS NOT NULL;  -- 方法2
```



------

## 十四、分组查询（GROUP BY）

### 基本格式

sql

```
SELECT 分组字段, 聚合函数(字段)
FROM 表名
[WHERE 分组前条件]
GROUP BY 分组字段
[HAVING 分组后条件];
```



> 💡 **核心理解**：分组后，聚合函数会对**每一组**分别进行计算。

### 练习示例

sql

```
-- 示例1：统计每个分类的商品数量
SELECT 
    category_id,           -- 分组字段
    COUNT(id) AS cnt       -- 每组分别计数
FROM products
GROUP BY category_id;

-- 示例2：统计每个分类中自营和非自营的商品数量（多字段分组）
SELECT 
    category_id,
    is_self,
    COUNT(id) AS cnt
FROM products
GROUP BY category_id, is_self;

-- 示例3：统计每个分类商品的平均价格，筛选平均价格低于1000的分类
SELECT 
    category_id,
    AVG(price) AS avg_price
FROM products
GROUP BY category_id
HAVING AVG(price) < 1000;

-- 示例4：统计自营商品中，每个分类的平均价格，筛选平均价格高于2000的分类
SELECT 
    category_id,
    AVG(price) AS avg_price
FROM products
WHERE is_self = '自营'        -- 先过滤（分组前）
GROUP BY category_id
HAVING AVG(price) > 2000;     -- 再过滤（分组后）
```



### WHERE 与 HAVING 的区别

| 对比项           | WHERE                  | HAVING         |
| :--------------- | :--------------------- | :------------- |
| 执行顺序         | 分组**前**过滤         | 分组**后**过滤 |
| 能否使用聚合函数 | ❌ 不能                 | ✅ 能           |
| 效率             | 更高（提前减少数据量） | 相对较低       |

------

## 十五、分页查询（LIMIT）

### 基本格式

sql

```
SELECT 字段名 FROM 表名 LIMIT 偏移量, 条数;
```



| 参数   | 说明                         |
| :----- | :--------------------------- |
| 偏移量 | 从第几条开始（0 表示第一条） |
| 条数   | 本次查询多少条               |

> 💡 **分页公式**：第 N 页，每页 M 条 → `LIMIT (N-1)*M, M`

### 练习示例

sql

```
-- 示例1：获取价格最高的商品
SELECT * FROM products 
ORDER BY price DESC 
LIMIT 1;                    -- 等价于 LIMIT 0, 1

-- 示例2：按价格升序，获取第2页数据（每页3条）
SELECT * FROM products 
ORDER BY price 
LIMIT 3, 3;                 -- 第1页：0,3；第2页：3,3

-- 示例3：分页数据不存在时不报错，返回空
SELECT * FROM products LIMIT 20, 10;  -- 结果：空
```



------

## 十六、SQL 执行顺序（重要）

### 书写顺序

sql

```
SELECT → DISTINCT → 聚合函数 → FROM → WHERE → GROUP BY → HAVING → ORDER BY → LIMIT
```



### 实际执行顺序

sql

```
FROM → WHERE → GROUP BY → 聚合函数 → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```



> 💡 **理解执行顺序有助于写出正确的 SQL**，例如：
>
> - `WHERE` 中不能用聚合函数，因为它在分组之前执行
> - 别名可以在 `ORDER BY` 中使用，但不能在 `WHERE` 中使用

------

# 第四部分：作业与答案

## 简答题

### 题目1：NULL 的含义

text

```
NULL 在数据库中表示「空值」或「未知值」。

要点：
1. NULL ≠ 0
2. NULL ≠ ''（空字符串）
3. NULL ≠ 'null'
4. 判断 NULL 必须用 IS NULL / IS NOT NULL
5. 聚合函数会忽略 NULL 值
```



### 题目2：WHERE 和 HAVING 的区别

text

```
1. 执行顺序不同：WHERE 在分组前，HAVING 在分组后
2. 聚合函数：WHERE 不能用，HAVING 能用
3. 效率：WHERE 效率更高（提前过滤数据）
```



------

## 实操题

### 题目1：基础查询练习

sql

```
USE db_hw2;

-- 1. 查询所有学生信息
SELECT * FROM student;

-- 2. 查询姓名和英语成绩
SELECT name, english FROM student;

-- 3. 去重查看语文成绩
SELECT DISTINCT chinese FROM student;

-- 4. 统计每个学生的总分
SELECT name, (chinese + english + math) AS 总分 FROM student;

-- 5. 总分加10分特长分
SELECT name, (chinese + english + math) + 10 AS 加分后总分 FROM student;

-- 6. 英语成绩大于90分的同学
SELECT name FROM student WHERE english > 90;

-- 7. 总分大于200分的同学
SELECT name FROM student WHERE (chinese + english + math) > 200;

-- 8. 英语分数在80-90之间
SELECT name FROM student WHERE english BETWEEN 80 AND 90;

-- 9. 英语分数不在80-90之间
SELECT name FROM student WHERE english NOT BETWEEN 80 AND 90;

-- 10. 数学分数为89,90,91的同学
SELECT * FROM student WHERE math IN (89, 90, 91);

-- 11. 姓李的学生英语成绩
SELECT name, english FROM student WHERE name LIKE '李%';

-- 12. 数学80且语文80的同学
SELECT * FROM student WHERE math = 80 AND chinese = 80;

-- 13. 英语80或者总分200的同学
SELECT * FROM student WHERE english = 80 OR (chinese + english + math) = 200;
```



------

### 题目2：students 表综合练习

sql

```
-- 1. 查询姓名为'百里守约'的学生
SELECT * FROM students WHERE name = '百里守约';

-- 2. 查询姓名为'百里守约'或'百里玄策'的学生
SELECT * FROM students WHERE name IN ('百里守约', '百里玄策');

-- 3. 查询姓'张'的学生
SELECT name, age, class_id FROM students WHERE name LIKE '张%';

-- 4. 查询姓名中包含'约'的学生
SELECT * FROM students WHERE name LIKE '%约%';

-- 5. 查询姓'孙'且名字为两个字的学生
SELECT studentNo, name, age, class_id, card FROM students WHERE name LIKE '孙__';

-- 6. 查询姓'百'或姓'孙'的学生
SELECT * FROM students WHERE name LIKE '百%' OR name LIKE '孙%';

-- 7. 查询姓'百'且家乡在'山西'的学生
SELECT * FROM students WHERE name LIKE '百%' AND hometown = '山西';

-- 8. 查询家乡在北京、新疆、山东、上海的学生
SELECT * FROM students WHERE hometown IN ('北京', '新疆', '山东', '上海');

-- 9. 查询姓'孙'且家乡不是'河北'的学生
SELECT * FROM students WHERE name LIKE '孙%' AND hometown != '河北';

-- 10. 查询家乡不在指定城市的学生
SELECT * FROM students WHERE hometown NOT IN ('北京', '新疆', '山东', '上海');

-- 11. 按性别排序
SELECT * FROM students ORDER BY sex;

-- 12. 查询男生并按年龄升序
SELECT * FROM students WHERE sex = '男' ORDER BY age;

-- 13. 统计学生总数
SELECT COUNT(*) FROM students;

-- 14. 统计年龄大于20岁的学生数量
SELECT COUNT(*) FROM students WHERE age > 20;

-- 15. 查询男生的平均年龄
SELECT AVG(age) FROM students WHERE sex = '男';

-- 16. 查询1班的最大年龄
SELECT MAX(age) FROM students WHERE class_id = 1;

-- 17. 统计2班男女各有多少人
SELECT class_id, sex, COUNT(*) FROM students WHERE class_id = 2 GROUP BY sex;

-- 18. 查询年龄最小的学生
SELECT * FROM students ORDER BY age LIMIT 1;
```



------

### 题目3：员工表（emp）综合练习

sql

```
-- 1. 按员工编号升序，查询不在10号部门的员工
SELECT *
FROM emp
WHERE deptno != 10
ORDER BY empno;

-- 2. 每个部门的平均薪水
SELECT deptno, AVG(sal) AS avg_sal
FROM emp
GROUP BY deptno;

-- 3. 各个部门的最高薪水
SELECT deptno, MAX(sal) AS max_sal
FROM emp
GROUP BY deptno;

-- 4. 每个部门每个岗位的最高薪水
SELECT deptno, job, MAX(sal) AS max_sal
FROM emp
GROUP BY deptno, job;

-- 5. 平均薪水大于2000的部门编号
SELECT deptno, AVG(sal)
FROM emp
GROUP BY deptno
HAVING AVG(sal) > 2000;

-- 6. 平均薪水大于1500的部门，按平均薪水降序
SELECT deptno, AVG(sal) AS avg_sal
FROM emp
GROUP BY deptno
HAVING avg_sal > 1500
ORDER BY avg_sal DESC;

-- 7. 姓名第二个字母不是"A"且薪水大于800，按年薪降序
-- IFNULL(comm, 0)：如果 comm 为 NULL，则当作 0 处理
SELECT *
FROM emp
WHERE ename NOT LIKE '_A%' AND sal > 800
ORDER BY (sal * 12 + IFNULL(comm, 0)) DESC;
```