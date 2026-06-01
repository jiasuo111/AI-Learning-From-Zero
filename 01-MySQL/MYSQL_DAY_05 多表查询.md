## 多表查询

> 💡 **核心思想**：把多个表通过主外键关联，使用 JOIN 连接合并为一个大的表，再进行查询。

------

## 十七、多表关系

### 1. 三种关系类型

| 关系类型   | 说明                           | 示例                     |
| :--------- | :----------------------------- | :----------------------- |
| **1对1**   | 一条记录对应另一张表的一条记录 | 一个人对应一张身份证     |
| **1对多**  | 一条记录对应另一张表的多条记录 | 一个老师对应多个学生     |
| **多对多** | 多条记录对应多条记录           | 课程和学生（需要中间表） |

### 2. 外键概念

- 从表的外键是对主表主键的引用
- 外键和主键的类型必须一致

text

```
主表（分类表）              从表（商品表）
┌─────────────┐           ┌─────────────────┐
│ id (主键)    │◄────────│ category_id     │
│ name        │   外键    │ (外键，引用主表) │
└─────────────┘           └─────────────────┘
```



------

## 十八、外键约束

### 1. 存储引擎要求

> ⚠️ **只有 InnoDB 存储引擎支持外键约束和事务**。MySQL 默认就是 InnoDB，一般无需修改。

### 2. 建表时添加外键

sql

```
-- 班级表（主表）
CREATE TABLE class(
    class_id INT PRIMARY KEY AUTO_INCREMENT,
    c_name VARCHAR(24) NOT NULL
);

-- 学生表（从表）
CREATE TABLE student(
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(10) NOT NULL,
    stu_class_id INT,
    -- 添加外键约束
    CONSTRAINT fk_student_class 
        FOREIGN KEY(stu_class_id) 
        REFERENCES class(class_id)
);
```



**语法说明：**

sql

```
CONSTRAINT 外键名字 FOREIGN KEY(从表字段) REFERENCES 主表名(主表主键)
```



### 3. 建表后添加/删除外键

sql

```
-- 添加外键
ALTER TABLE 从表名 
ADD CONSTRAINT 外键约束名 
FOREIGN KEY (外键名) 
REFERENCES 主表名 (主表主键);

-- 删除外键
ALTER TABLE 从表名 DROP FOREIGN KEY 外键约束名;
```



### 4. 外键约束的作用

| 约束类型     | 作用                                                 |
| :----------- | :--------------------------------------------------- |
| 限制从表插入 | 从表插入时，外键值必须在主表主键中存在，否则插入失败 |
| 限制主表删除 | 主表删除时，如果主键值被从表外键引用，则删除失败     |

> 💡 **作用**：保证数据的准确性和完整性。

### 5. 完整示例

sql

```
-- 创建分类表（主表）
CREATE TABLE category1 (
    cid VARCHAR(32) PRIMARY KEY,
    cname VARCHAR(100)
);

-- 创建商品表（从表，带外键）
CREATE TABLE products1 (
    pid VARCHAR(32) PRIMARY KEY,
    pname VARCHAR(40),
    price DOUBLE,
    category_id VARCHAR(32),
    CONSTRAINT fk_products_category 
        FOREIGN KEY (category_id) 
        REFERENCES category1 (cid)
);

-- 删除外键
ALTER TABLE products1 DROP FOREIGN KEY fk_products_category;

-- 添加外键
ALTER TABLE products1 
ADD CONSTRAINT fk_new 
FOREIGN KEY (category_id) REFERENCES category1 (cid);

-- 插入数据演示约束
INSERT INTO products1 VALUES('p1','小米',999,'c001'); -- 失败，c001不存在

INSERT INTO category1 VALUES('c001','手机'); -- 先插入主表
INSERT INTO products1 VALUES('p1','小米',999,'c001'); -- 成功

-- 删除数据演示约束
DELETE FROM category1 WHERE cid='c001'; -- 失败，已被引用

-- 解决方案：先解除引用
UPDATE products1 SET category_id = NULL WHERE category_id = 'c001';
DELETE FROM category1 WHERE cid='c001'; -- 成功
```



------

## 十九、连接查询（数据准备）

sql

```
USE day03_db;

-- 商品表
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(24) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    score DECIMAL(5, 2),
    is_self VARCHAR(8),
    category_id INT
);

-- 分类表
CREATE TABLE category (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(24) NOT NULL
);

-- 插入数据
INSERT INTO category VALUES (1, '手机'), (2, '电脑'), (3, '美妆'), (4, '家居');

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
(12, '迪奥996', 330.00, 9.70, '非自营', 3),
(13, '百草味紫皮腰果', 9, NULL, NULL, NULL),
(14, '奥利奥', 8, NULL, NULL, 5);
```



------

## 二十、交叉连接（CROSS JOIN）

### 语法

sql

```
-- 显式交叉连接
SELECT * FROM 表1 CROSS JOIN 表2;

-- 隐式交叉连接（危险！）
SELECT * FROM 表1, 表2;
```



> ⚠️ **警告**：交叉连接会产生「笛卡尔积」，即两个表的数据相乘。如果两个表各有100万条数据，结果将是1万亿条，可能导致电脑崩溃！**谨慎使用！**

------

## 二十一、内连接（INNER JOIN）

### 语法

sql

```
-- 显式内连接（推荐）
SELECT * FROM 表1 INNER JOIN 表2 ON 关联条件;

-- 隐式内连接
SELECT * FROM 表1, 表2 WHERE 关联条件;
```



### 特点

- 取两个表的**交集**
- 只返回满足关联条件的数据

### 示例

sql

```
-- 隐式内连接
SELECT c.id AS cid, c.name AS cname, p.id AS pid, p.name AS pname
FROM products p, category c
WHERE p.category_id = c.id;

-- 显式内连接
SELECT c.id AS cid, c.name AS cname, p.id AS pid, p.name AS pname
FROM products p
INNER JOIN category c ON p.category_id = c.id;
```



------

## 二十二、外连接

### 1. 左外连接（LEFT OUTER JOIN）

**语法：**

sql

```
SELECT * FROM 左表 LEFT OUTER JOIN 右表 ON 关联条件;
```



**特点：** 左表的**全部数据**都会显示，即使右表中没有匹配的数据（右表字段显示为 NULL）。

### 2. 右外连接（RIGHT OUTER JOIN）

**语法：**

sql

```
SELECT * FROM 左表 RIGHT OUTER JOIN 右表 ON 关联条件;
```



**特点：** 右表的**全部数据**都会显示，即使左表中没有匹配的数据。

> 💡 `OUTER` 关键字可以省略，直接写 `LEFT JOIN` 或 `RIGHT JOIN`。

### 示例

sql

```
-- 插入一条没有对应分类的商品
INSERT INTO products(name, price, category_id) VALUES ('百草味紫皮腰果', 9, 5);

-- 左外连接：以分类表为主，显示所有分类
SELECT c.id AS cid, c.name AS cname, p.id AS pid, p.name AS pname
FROM category c
LEFT OUTER JOIN products p ON p.category_id = c.id;

-- 右外连接：同样效果（交换表位置）
SELECT c.id AS cid, c.name AS cname, p.id AS pid, p.name AS pname
FROM products p
RIGHT OUTER JOIN category c ON p.category_id = c.id;
```



------

## 二十三、全外连接（FULL OUTER JOIN）

> ⚠️ MySQL **不支持** `FULL OUTER JOIN`，需要使用 `UNION` 或 `UNION ALL` 模拟。

### 语法

sql

```
-- UNION：自动去重
SELECT * FROM 左表 LEFT JOIN 右表 ON 关联条件
UNION
SELECT * FROM 左表 RIGHT JOIN 右表 ON 关联条件;

-- UNION ALL：不去重
SELECT * FROM 左表 LEFT JOIN 右表 ON 关联条件
UNION ALL
SELECT * FROM 左表 RIGHT JOIN 右表 ON 关联条件;
```



### 示例

sql

```
-- 全外连接（去重版）
SELECT * FROM products p LEFT JOIN category c ON p.category_id = c.id
UNION
SELECT * FROM products p RIGHT JOIN category c ON p.category_id = c.id;
```



------

## 二十四、自连接查询

### 概念

当一张表与自己关联时，称为**自连接**。例如：员工表中既有员工信息，又有员工对应的领导信息（领导也是员工）。

### 技巧

> 💡 把同一张表**想象成两张不同的表**，分别起别名，然后进行连接。

### 示例

sql

```
-- 员工表结构示例
CREATE TABLE all_id (
    id INT PRIMARY KEY,      -- 自己的ID
    name VARCHAR(20),        -- 自己的名字
    pid INT,                 -- 领导的ID
    pname VARCHAR(20)        -- 领导的名字
);

-- 自连接查询：查询每个员工及其领导
SELECT 
    yuangong.id AS 员工ID,
    yuangong.name AS 员工姓名,
    lingdao.pname AS 领导姓名
FROM all_id yuangong
JOIN all_id lingdao ON yuangong.pid = lingdao.id;
```



------

## 二十五、子查询

### 概念

在一个 `SELECT` 语句中**嵌套**另一个 `SELECT` 语句。当条件难以一步到位时，可以分步处理。

### 1. 子查询作为条件（WHERE 中使用）

sql

```
-- 需求：查询价格大于平均价的商品
SELECT * FROM products
WHERE price > (SELECT AVG(price) FROM products);

-- 需求：查询价格最高的商品（考虑并列情况）
SELECT * FROM products
WHERE price = (SELECT MAX(price) FROM products);
```



### 2. 子查询作为表（FROM 中使用）

> ⚠️ 子查询作为表时，**必须加括号并起别名**！

sql

```
-- 需求：查询各个分类中商品的平均价格，要求结果包含分类名称
-- 思路：先查各分类平均价格，再关联分类表
SELECT c.name, t.avg_price
FROM (SELECT category_id, AVG(price) AS avg_price 
      FROM products 
      GROUP BY category_id) t
JOIN category c ON t.category_id = c.id;
```



### 3. 子查询作为字段（SELECT 中使用）

sql

```
-- 需求：计算每个学生的分数和整体平均分的差值
SELECT
    id,
    name,
    score,
    (SELECT ROUND(AVG(score), 2) FROM students) AS avg_score,
    score - (SELECT ROUND(AVG(score), 2) FROM students) AS diff
FROM students;
```



------

# 第七部分：多表查询作业答案

## 实操题1：用户-订单-商品综合练习

### 数据准备

sql

```
CREATE DATABASE db_hw3;
USE db_hw3;

-- 用户表
CREATE TABLE USER (
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(20),
    age INT
);
INSERT INTO USER VALUES (1,'张三',23),(2,'李四',24),(3,'王五',25),(4,'赵六',26);

-- 订单表
CREATE TABLE orderlist (
    id INT PRIMARY KEY AUTO_INCREMENT,
    number VARCHAR(30),
    uid INT,
    CONSTRAINT ou_fk1 FOREIGN KEY (uid) REFERENCES USER(id)
);
INSERT INTO orderlist VALUES 
(1,'hm001',1),(2,'hm002',1),(3,'hm003',2),(4,'hm004',2),
(5,'hm005',3),(6,'hm006',3),(7,'hm007',NULL);

-- 商品分类表
CREATE TABLE category (
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(10)
);
INSERT INTO category VALUES (1,'手机数码'),(2,'电脑办公'),(3,'烟酒茶糖'),(4,'鞋靴箱包');

-- 商品表
CREATE TABLE product (
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(30),
    cid INT,
    CONSTRAINT cp_fk1 FOREIGN KEY (cid) REFERENCES category(id)
);
INSERT INTO product VALUES 
(1,'华为手机',1),(2,'小米手机',1),(3,'联想电脑',2),(4,'苹果电脑',2),
(5,'中华香烟',3),(6,'玉溪香烟',3),(7,'计生用品',NULL);

-- 中间表（用户-商品）
CREATE TABLE us_pro (
    upid INT PRIMARY KEY AUTO_INCREMENT,
    uid INT,
    pid INT,
    CONSTRAINT up_fk1 FOREIGN KEY (uid) REFERENCES USER(id),
    CONSTRAINT up_fk2 FOREIGN KEY (pid) REFERENCES product(id)
);
-- 插入数据（每个用户都能看到所有商品）
INSERT INTO us_pro VALUES (NULL,1,1),(NULL,1,2),(NULL,1,3),(NULL,1,4),(NULL,1,5),(NULL,1,6),(NULL,1,7);
INSERT INTO us_pro VALUES (NULL,2,1),(NULL,2,2),(NULL,2,3),(NULL,2,4),(NULL,2,5),(NULL,2,6),(NULL,2,7);
INSERT INTO us_pro VALUES (NULL,3,1),(NULL,3,2),(NULL,3,3),(NULL,3,4),(NULL,3,5),(NULL,3,6),(NULL,3,7);
INSERT INTO us_pro VALUES (NULL,4,1),(NULL,4,2),(NULL,4,3),(NULL,4,4),(NULL,4,5),(NULL,4,6),(NULL,4,7);
```



### 题目1-5：用户与订单

sql

```
-- 1. 查询用户的编号、姓名、年龄、订单编号（有订单的用户）
SELECT u.id AS 用户编号, u.name AS 姓名, u.age AS 年龄, o.number AS 订单编号
FROM USER u, orderlist o
WHERE u.id = o.uid;

-- 2. 查询所有的用户（包括没有订单的）
SELECT u.id AS 用户编号, u.name AS 姓名, u.age AS 年龄, o.number AS 订单编号
FROM USER u LEFT JOIN orderlist o ON u.id = o.uid;

-- 3. 查询所有的订单（包括没有用户的订单）
SELECT u.id AS 用户编号, u.name AS 姓名, u.age AS 年龄, o.number AS 订单编号
FROM orderlist o LEFT JOIN USER u ON u.id = o.uid;

-- 4. 查询用户年龄大于23岁的信息
SELECT u.id AS 用户编号, u.name AS 姓名, u.age AS 年龄, o.number AS 订单编号
FROM USER u, orderlist o
WHERE u.id = o.uid AND u.age > 23;

-- 5. 查询张三和李四用户的信息
SELECT u.id AS 用户编号, u.name AS 姓名, u.age AS 年龄, o.number AS 订单编号
FROM USER u JOIN orderlist o ON u.id = o.uid
WHERE u.name IN ('张三', '李四');
```



### 题目6-10：分类、商品、用户

sql

```
-- 6. 查询商品分类的编号、分类名称、分类下的商品名称
SELECT c.id AS 分类编号, c.name AS 分类名称, p.name AS 商品名称
FROM category c, product p
WHERE c.id = p.cid;

-- 7. 查询所有的商品分类（包括没有商品的）
SELECT c.id AS 分类编号, c.name AS 分类名称, p.name AS 商品名称
FROM category c LEFT JOIN product p ON c.id = p.cid;

-- 8. 查询所有的商品信息（包括没有分类的）
SELECT p.name AS 商品名称, c.id AS 分类编号, c.name AS 分类名称
FROM product p LEFT JOIN category c ON c.id = p.cid;

-- 9. 查询用户信息和该用户能查看的所有商品
SELECT u.id AS 用户编号, u.name AS 姓名, u.age AS 年龄, p.name AS 商品名称
FROM us_pro up, USER u, product p
WHERE up.uid = u.id AND up.pid = p.id;

-- 10. 查询张三和李四这两个用户可以看到的商品
SELECT u.id AS 用户编号, u.name AS 姓名, u.age AS 年龄, p.name AS 商品名称
FROM us_pro up, USER u, product p
WHERE up.uid = u.id AND up.pid = p.id AND u.name IN ('张三', '李四');
```



------

## 实操题2：员工表综合练习

### 数据准备

sql

```
-- 部门表
CREATE TABLE dept (
    deptno INT PRIMARY KEY,
    dname VARCHAR(14),
    loc VARCHAR(13)
);
INSERT INTO dept VALUES 
(10,'accounting','new york'),
(20,'research','dallas'),
(30,'sales','chicago'),
(40,'operations','boston');

-- 员工表
CREATE TABLE emp (
    empno INT PRIMARY KEY,
    ename VARCHAR(10),
    job VARCHAR(9),
    mgr INT,
    hiredate DATE,
    sal DOUBLE,
    comm DOUBLE,
    deptno INT
);
INSERT INTO emp VALUES
(7369,'smith','职员',7566,'1980-12-17',800,null,20),
(7499,'allen','销售员',7698,'1981-02-20',1600,300,30),
(7521,'ward','销售员',7698,'1981-02-22',1250,500,30),
(7566,'jones','经理',7839,'1981-04-02',2975,null,20),
(7654,'martin','销售员',7698,'1981-09-28',1250,1400,30),
(7698,'blake','经理',7839,'1981-05-01',2850,null,30),
(7782,'clark','经理',7839,'1981-06-09',2450,null,10),
(7788,'scott','职员',7566,'1987-07-03',3000,2000,20),
(7839,'king','董事长',null,'1981-11-17',5000,null,10),
(7844,'turners','销售员',7698,'1981-09-08',1500,50,30),
(7876,'adams','职员',7566,'1987-07-13',1100,null,20),
(7900,'james','职员',7698,'1981-12-03',1250,null,30),
(7902,'ford','销售员',7566,'1981-12-03',3000,null,20),
(7934,'miller','职员',7782,'1981-01-23',1300,null,10);

-- 工资等级表
CREATE TABLE salgrade (
    grade INT,
    losal DOUBLE,
    hisal DOUBLE
);
INSERT INTO salgrade VALUES 
(1,500,1000),
(2,1001,1500),
(3,1501,2000),
(4,2001,3000),
(5,3001,9999);
```



### 练习题答案

sql

```
-- 1. 返回部门号及其本部门的最低工资
SELECT deptno, MIN(sal) FROM emp GROUP BY deptno ORDER BY deptno;

-- 2. 计算出员工的年薪，并且以年薪降序排序
SELECT ename, sal*12 + IFNULL(comm,0) AS 年薪 
FROM emp ORDER BY 年薪 DESC;

-- 3. 返回所有员工各个job工作的最低工资
SELECT job, MIN(sal) FROM emp GROUP BY job;

-- 4. 查找和scott从事相同工作的员工信息
SELECT * FROM emp WHERE job = (SELECT job FROM emp WHERE ename = 'scott');

-- 5. 工资水平多于james的员工信息
SELECT * FROM emp WHERE sal > (SELECT sal FROM emp WHERE ename = 'james');

-- 6. 返回工资大于平均工资的员工信息
SELECT * FROM emp WHERE sal > (SELECT AVG(sal) FROM emp);

-- 7. 返回销售部(sales)所有员工的姓名
SELECT * FROM emp WHERE deptno = (SELECT deptno FROM dept WHERE dname = 'sales');

-- 8. 返回工资高于30部门所有员工工资水平的员工信息
SELECT * FROM emp WHERE sal > (SELECT MAX(sal) FROM emp WHERE deptno = 30);

-- 9. 返回查找最高工资和最低工资的职员信息
SELECT * FROM emp e
WHERE (e.sal = (SELECT MAX(sal) FROM emp WHERE job = '职员') 
    OR e.sal = (SELECT MIN(sal) FROM emp WHERE job = '职员'))
    AND e.job = '职员';

-- 10. 返回拥有员工的部门名、部门号
SELECT d.deptno, d.dname FROM dept d
WHERE d.deptno IN (SELECT e.deptno FROM emp e WHERE e.deptno IS NOT NULL);

-- 11. 返回员工的姓名、所在部门名及其工资
SELECT emp.ename, dept.dname, emp.sal FROM emp, dept WHERE emp.deptno = dept.deptno;

-- 12. 返回从事职员工作的员工姓名和所在部门名称
SELECT emp.ename, dept.dname
FROM emp, dept
WHERE emp.deptno = dept.deptno AND emp.job = '职员';

-- 13. 返回部门号、部门名、部门所在位置及其每个部门的员工总数
SELECT dept.deptno, dept.dname, dept.loc, COUNT(*)
FROM dept, emp
WHERE dept.deptno = emp.deptno
GROUP BY dept.deptno;

-- 14. 返回所有员工（职员或者销售员）和所属经理的姓名
SELECT e1.ename AS 员工, e2.ename AS 经理
FROM (SELECT * FROM emp WHERE job IN ('职员','销售员')) e1,
     (SELECT * FROM emp) e2
WHERE e1.mgr = e2.empno;

-- 15. 查询所有员工姓名、职位、薪水、薪资等级
SELECT e.ename AS 员工姓名, e.job AS 职位, e.sal AS 薪水, sg.grade AS 薪资等级
FROM emp e, salgrade sg
WHERE e.sal BETWEEN sg.losal AND sg.hisal;
```



------

# 阶段总结

## 多表查询核心知识点

| 连接类型 | 关键字         | 结果                |
| :------- | :------------- | :------------------ |
| 内连接   | `INNER JOIN`   | 两表交集            |
| 左外连接 | `LEFT JOIN`    | 左表全部 + 右表匹配 |
| 右外连接 | `RIGHT JOIN`   | 右表全部 + 左表匹配 |
| 全外连接 | `UNION` 模拟   | 两表全部            |
| 自连接   | 同一张表起别名 | 自己关联自己        |
| 子查询   | `SELECT` 嵌套  | 分步查询            |