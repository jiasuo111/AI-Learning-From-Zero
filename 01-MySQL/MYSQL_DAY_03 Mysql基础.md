## 一、数据库概述

### 1. 数据库概念

- 数据库：存储数据的仓库，本质上是一个文件系统。

### 2. 数据库分类

#### 2.1 关系型数据库（SQL）

- 必须遵循SQL规范，强调以二维表格的形式存储数据。
- 例如：MySQL、Oracle、DB2、SQL Server、SQLite。

#### 2.2 非关系型数据库（NoSQL）

- 不仅仅是SQL，强调以键值对（key-value）的形式存储数据。
- 例如：HBase、Redis、MongoDB。

------

## 二、数据库连接（命令行方式）

1. 打开命令行：

   - `Win + R` → 输入 `cmd` → 回车
   - 或直接在Win10任务栏搜索 `cmd`

2. 连接MySQL：

   sql

   ```
   mysql -u用户名 -p密码
   ```

   

   示例（默认用户名和密码都是root）：

   sql

   ```
   mysql -uroot -proot
   ```

   

3. 成功连接后，命令行提示符变为 `mysql>`，表示已进入数据库环境。

> 注：此方法仅作为了解，实际开发中较少使用命令行操作数据库。

------

## 三、SQL 规范

### 1. SQL 概念

- SQL：结构化查询语言，是所有关系型数据库必须遵守的规范。
- 类比：SQL 是普通话，MySQL、Oracle 等是方言。

### 2. SQL 语言分类

| 分类 | 全称                       | 作用                         | 常用关键字                   |
| :--- | :------------------------- | :--------------------------- | :--------------------------- |
| DDL  | Data Definition Language   | 定义数据库对象（库、表、列） | `CREATE`, `DROP`, `ALTER`    |
| DML  | Data Manipulation Language | 操作表中数据（增删改）       | `INSERT`, `DELETE`, `UPDATE` |
| DQL  | Data Query Language        | 查询表中数据                 | `SELECT`, `FROM`, `WHERE`    |
| DCL  | Data Control Language      | 控制访问权限、安全级别、用户 | `GRANT`, `REVOKE`            |

------

## 四、数据库的增删改查操作

| 操作                 | 命令                                        |
| :------------------- | :------------------------------------------ |
| 创建数据库           | `CREATE DATABASE [IF NOT EXISTS] 数据库名;` |
| 删除数据库           | `DROP DATABASE [IF EXISTS] 数据库名;`       |
| 使用数据库           | `USE 数据库名;`                             |
| 查看所有数据库       | `SHOW DATABASES;`                           |
| 查看当前使用的数据库 | `SELECT DATABASE();`                        |
| 查看建库语句         | `SHOW CREATE DATABASE 数据库名;`            |

------

## 五、数据类型与约束

### 1. 常用数据类型

| 类型                                        | 说明               |
| :------------------------------------------ | :----------------- |
| `VARCHAR(长度)`                             | 字符串             |
| `INT`                                       | 整型（默认长度11） |
| `FLOAT` / `DOUBLE` / `DECIMAL(总长,小数位)` | 浮点数             |
| `DATE` / `TIME` / `DATETIME` / `YEAR`       | 日期时间           |

### 2. 常见约束

| 约束     | 关键字           | 说明                           |
| :------- | :--------------- | :----------------------------- |
| 主键约束 | `PRIMARY KEY`    | 值唯一且不为空，每表只能有一个 |
| 主键自增 | `AUTO_INCREMENT` | 自动递增                       |
| 非空约束 | `NOT NULL`       | 不能为空                       |
| 唯一约束 | `UNIQUE`         | 值唯一                         |
| 默认约束 | `DEFAULT 值`     | 未指定时使用默认值             |

------

## 六、数据表的增删改查操作

### 1. 创建表

sql

```
CREATE TABLE 表名 (
    字段名 数据类型 约束,
    ...
);
```



示例：

sql

```
CREATE TABLE stu_tb (
    id INT PRIMARY KEY,
    name VARCHAR(10) NOT NULL
);
```



### 2. 常用表操作

| 操作         | 命令                             |
| :----------- | :------------------------------- |
| 删除表       | `DROP TABLE 表名;`               |
| 修改表名     | `RENAME TABLE 旧表名 TO 新表名;` |
| 查看所有表   | `SHOW TABLES;`                   |
| 查看建表语句 | `SHOW CREATE TABLE 表名;`        |

------

## 七、修改表中的字段（列）

| 操作            | 命令                                                     |
| :-------------- | :------------------------------------------------------- |
| 添加字段        | `ALTER TABLE 表名 ADD [COLUMN] 字段名 类型 [约束];`      |
| 删除字段        | `ALTER TABLE 表名 DROP 字段名;`                          |
| 修改字段名+类型 | `ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型 [约束];` |
| 修改类型/约束   | `ALTER TABLE 表名 MODIFY 字段名 类型 [约束];`            |
| 查看表结构      | `DESC 表名;`                                             |

------

## 八、表中记录操作（DML）

### 1. 插入数据

sql

```
INSERT INTO 表名 (字段1, 字段2) VALUES (值1, 值2), (值1, 值2);
```



### 2. 修改数据

sql

```
UPDATE 表名 SET 字段 = 值 [WHERE 条件];
```



> 如果不加 WHERE，会更新整张表。

### 3. 删除数据

sql

```
DELETE FROM 表名 [WHERE 条件];
```



### 4. 清空整表

sql

```
TRUNCATE 表名;
```



------

# 作业答案

## 简答题

### 题目1：数据库的作用与常见数据库

text

```
数据库作用：存储和管理数据，如商品信息、用户信息等。
常见数据库：
    关系型：MySQL、Oracle、SQL Server
    非关系型：HBase、Redis
```



### 题目2：SQL语言分类及关键字

text

```
1. DDL（数据定义语言）：CREATE、ALTER、DROP
2. DML（数据操作语言）：INSERT、UPDATE、DELETE
3. DQL（数据查询语言）：SELECT、FROM、WHERE
4. DCL（数据控制语言）：GRANT、REVOKE
```



------

## 实操题

### 题目1：数据库操作

sql

```
-- (1) 创建数据库 db_student1
CREATE DATABASE IF NOT EXISTS db_student1;

-- (2) 创建数据库 db_hw1
CREATE DATABASE IF NOT EXISTS db_hw1;

-- (3) 切换到 db_student1
USE db_student1;

-- (4) 查看当前数据库
SELECT DATABASE();

-- (5) 查看所有数据库
SHOW DATABASES;

-- (6) 删除 db_student1
DROP DATABASE db_student1;

-- (7) 再次查看当前数据库
SELECT DATABASE();

-- (8) 切换到 db_hw1
USE db_hw1;
```



------

### 简答题：约束的作用与分类

text

```
约束作用：限制输入数据的规则，保证数据的完整性和一致性。
常见约束：
    PRIMARY KEY：主键，唯一且非空
    AUTO_INCREMENT：自增
    NOT NULL：非空
    UNIQUE：唯一
    DEFAULT：默认值
```



------

### 实操题：数据表操作（题目1）

sql

```
-- (1) 创建 student 表
USE db_hw1;
CREATE TABLE student (
    sid INT,
    name VARCHAR(20),
    sex VARCHAR(5),
    birthday DATE
);

-- (2) 查看字段信息
DESC student;

-- (3) 查看建表语句
SHOW CREATE TABLE student;

-- (4) 创建 little_student 表
CREATE TABLE little_student (
    name VARCHAR(20)
);

-- (5) 查看所有表
SHOW TABLES;

-- (6) 删除 little_student 表
DROP TABLE little_student;

-- (7) 修改表名
RENAME TABLE student TO tb_student;

-- (8) 再次查看所有表
SHOW TABLES;
```



------

### 实操题：数据表操作（题目2）

sql

```
-- (0) 创建数据库 db_hw2
CREATE DATABASE IF NOT EXISTS db_hw2;
USE db_hw2;

-- (1) 创建 tb_student 表（如果存在则使用已有）
CREATE TABLE IF NOT EXISTS tb_student (
    sid INT,
    name VARCHAR(20),
    sex VARCHAR(5),
    birthday DATE
);

-- (2) 添加 age 字段
ALTER TABLE tb_student ADD age INT;

-- (3) 添加 desc 字段
ALTER TABLE tb_student ADD `desc` VARCHAR(200);

-- (4) 查看所有字段
DESC tb_student;

-- (5) 修改 birthday 字段
ALTER TABLE tb_student CHANGE birthday birth DATETIME;
DESC tb_student;

-- (6) 删除 desc 字段
ALTER TABLE tb_student DROP `desc`;
DESC tb_student;
```



------

### 实操题：DML 操作（题目3）

sql

```
-- (1) 使用数据库 db_hw2
USE db_hw2;

-- (2)(3) 创建 student 表
CREATE TABLE student (
    编号 INT PRIMARY KEY AUTO_INCREMENT,
    学号 VARCHAR(20),
    姓名 VARCHAR(20),
    语文成绩 INT,
    英语成绩 INT,
    数学成绩 INT
);

-- (4) 插入数据
INSERT INTO student (学号, 姓名, 语文成绩, 英语成绩, 数学成绩) VALUES
('20210908001', '张王明', 89, 78, 90),
('20210908002', '李进', 67, 53, 95),
('20210908003', '王俊', 87, 78, 77),
('20210908004', '李云云', 80, 98, 92),
('20210908005', '谢来财', 82, 84, 67),
('20210908006', '张进宝', 55, 85, 89),
('20210908007', '黄蓉儿', 79, 86, 90),
('20210908008', '刘小雪', 71, 90, 91),
('20210908009', '夏金章', 89, 91, 96),
('20210908010', '杨洋', 83, 65, 90);

-- (5) 修改和删除数据
UPDATE student SET 语文成绩 = 88 WHERE 编号 = 4;
DELETE FROM student WHERE 编号 = 10;
```