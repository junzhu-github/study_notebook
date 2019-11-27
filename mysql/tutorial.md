<!--
 * @Date: 2019-10-24 11:12:53
 * @Author: YING
 * @LastEditTime: 2019-10-24 16:31:29
 -->

# MYSQL学习记录

## 数据库操作

```mysql
# 新建数据库
CREATE DATABASE [IF NOT EXISTS] db_name;

# 示例
CREATE DATABASE IF NOT EXISTS school;

------------------------------------------------------------------------------------
# 查看数据库
SHOW DATABASES;

# 查看当前数据库
SELECT DATABASE();
------------------------------------------------------------------------------------
# 进入数据库
USE db_name；

------------------------------------------------------------------------------------
# 删除数据库
DROP DATABASE db_name
```

## 表格操作

### 创建表格

```mysql
# 新建表格
CREATE DATABASE [IF NOT EXISTS] db_name;

# 示例
CREATE TABLE IF NOT EXISTS students(
    id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY, -- 序号，主键：每表唯一，非空
    userid INT UNSIGNED,    -- 学号
    username VARCHAR(20) NOT NULL,   -- 姓名
    age TINYINT UNSIGNED,   -- 年龄
    gender ENUM('0','1') DEFAULT '0',   -- 性别,默认女性
    class INT UNSIGNED   -- 班级
);

CREATE TABLE IF NOT EXISTS scores(
    id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY, -- 序号
    userid INT UNSIGNED,    -- 学号
    course VARCHAR(50) NOT NULL,   -- 课程
    score TINYINT UNSIGNED   -- 分数
);
```

![数据类型表](数据类型表.png)

### 查看表格

```mysql
# 查看库里面所有表格
SHOW TABLES [FROM db_name];

# 示例
SHOW TABLES FROM school；
--->
+───────────────────+
| Tables_in_school  |
+───────────────────+
| scores            |
| students          |
+───────────────────+

------------------------------------------------------------------------------------
# 查看当前表的数据结构
SHOW COLUMNS FROM tb_name;
或者
DESC tb_name;

# 示例
DESC students;
--->
+───────────+───────────────────────+───────+──────+──────────+─────────────────+
| Field     | Type                  | Null  | Key  | Default  | Extra           |
+───────────+───────────────────────+───────+──────+──────────+─────────────────+
| id        | smallint(5) unsigned  | NO    | PRI  |          | auto_increment  |
| userid    | int(10) unsigned      | YES   |      |          |                 |
| username  | varchar(20)           | NO    |      |          |                 |
| age       | tinyint(3) unsigned   | YES   |      |          |                 |
| gender    | enum('0','1')         | YES   |      | 0        |                 |
| class     | int(10) unsigned      | YES   |      |          |                 |
+───────────+───────────────────────+───────+──────+──────────+─────────────────+
```

### 修改表格

```mysql
# 修改表名称
ALTER TABLE tb_name RENAME new_tb_name

------------------------------------------------------------------------------------
# 增加列
ALTER TABLE tb_name
ADD COLUMN col_name [字段约束] [FIRST添加到所有列字段前/AFTER col添加到列字段之后],
ADD COLUMN col_name [字段约束] [FIRST添加到所有列字段前/AFTER col添加到列字段之后];

# 示例
ALTER TABLE students
ADD COLUMN col1 VARCHAR(255) NOT NULL FIRST,    -- 增加col1到所有列的最前
ADD COLUMN col2 INT NOT NULL AFTER age;    -- 增加col2到age列后面
--->
+───────────+───────────────────────+───────+──────+──────────+─────────────────+
| Field     | Type                  | Null  | Key  | Default  | Extra           |
+───────────+───────────────────────+───────+──────+──────────+─────────────────+
| col1      | varchar(255)          | NO    |      |          |                 |
| id        | smallint(5) unsigned  | NO    | PRI  |          | auto_increment  |
| userid    | int(10) unsigned      | YES   |      |          |                 |
| username  | varchar(20)           | NO    |      |          |                 |
| age       | tinyint(3) unsigned   | YES   |      |          |                 |
| col2      | int(11)               | NO    |      |          |                 |
| gender    | enum('0','1')         | YES   |      | 0        |                 |
| class     | int(10) unsigned      | YES   |      |          |                 |
+───────────+───────────────────────+───────+──────+──────────+─────────────────+

------------------------------------------------------------------------------------
# 修改列名称和约束类型
ALTER TABLE tb_name
CHANGE col_name new_col_name CONSTRAINT;

# 示例
ALTER TABLE students
CHANGE col1 new_col1 INT NOT NULL;  -- 同时修改列名称和约束类型
--->
+───────────+───────────────────────+───────+──────+──────────+─────────────────+
| Field     | Type                  | Null  | Key  | Default  | Extra           |
+───────────+───────────────────────+───────+──────+──────────+─────────────────+
| new_col1  | int(11)               | YES   |      |          |                 |
| id        | smallint(5) unsigned  | NO    | PRI  |          | auto_increment  |

------------------------------------------------------------------------------------
# 只修改列的约束类型
ALTER TABLE tb_name
MODIFY col_name CONSTRAINT;

# 示例
ALTER TABLE students
MODIFY new_col1 tinyint(3);
--->
+───────────+───────────────────────+───────+──────+──────────+─────────────────+
| Field     | Type                  | Null  | Key  | Default  | Extra           |
+───────────+───────────────────────+───────+──────+──────────+─────────────────+
| new_col1  | tinyint(3)            | YES   |      |          |                 |
| id        | smallint(5) unsigned  | NO    | PRI  |          | auto_increment  |

------------------------------------------------------------------------------------
# 删除列
ALTER TABLE tb_name
DROP col_name,DROP col_name,......;

# 示例
ALTER TABLE students
DROP new_col1,
DROP col2;
--->
+───────────+───────────────────────+───────+──────+──────────+─────────────────+
| Field     | Type                  | Null  | Key  | Default  | Extra           |
+───────────+───────────────────────+───────+──────+──────────+─────────────────+
| id        | smallint(5) unsigned  | NO    | PRI  |          | auto_increment  |
| userid    | int(10) unsigned      | YES   |      |          |                 |
| username  | varchar(20)           | NO    |      |          |                 |
| age       | tinyint(3) unsigned   | YES   |      |          |                 |
| gender    | enum('0','1')         | YES   |      | 0        |                 |
| class     | int(10) unsigned      | YES   |      |          |                 |
+───────────+───────────────────────+───────+──────+──────────+─────────────────+
```

### 删除表格

```mysql
DROP TABLE tb_name;
```

## 数据操作


