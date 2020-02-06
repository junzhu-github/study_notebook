<!--
 * @Date: 2019-10-24 11:12:53
 * @Author: YING
 * @LastEditTime : 2020-02-05 17:51:52
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

```mysql
# 复制表格结构体
CREATE DATABASE [IF NOT EXISTS] db_name LIKE db;

# 示例
CREATE TABLE IF NOT EXISTS copy_students(
LIKE students;

-----------------------------------------------

# 复制表格结构体 + 数据
CREATE DATABASE [IF NOT EXISTS] db_name 
AS SELECT * FROM db;

# 示例
CREATE TABLE IF NOT EXISTS copy_students(
AS SELECT * FROM students;
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
DROP col_name,DROP col_name,... ;

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

------------------------------------------------------------------------------------
# 删除主键
ALTER TABLE tb_name Drop PRIMARY KEY;

# 增加主键
ALTER TABLE tb_name ADD PRIMARY KEY(id);

------------------------------------------------------------------------------------
# 增加索引
ALTER TABLE tb_name ADD INDEX indx_name (col_name) ;
```

### 删除表格

```mysql
DROP TABLE tb_name;
```

## 数据操作

### 添加数据

```mysql
# 添加数据
INSERT INTO TABLE tb_name VALUES (),(),();

# 示例
INSERT INTO  students VALUES (DEFAULT,1,"jack",10,'0',1),(DEFAULT,2,"emily",10,'1',2);
--->
+─────+─────────+───────────+──────+─────────+────────+
| id  | userid  | username  | age  | gender  | class  |
+─────+─────────+───────────+──────+─────────+────────+
| 1   | 1       | jack      | 10   | 0       | 1      |
| 2   | 2       | emily     | 10   | 1       | 2      |
+─────+─────────+───────────+──────+─────────+────────+

1.对于自动编号的字段，插入“NULL”或“DEFAULT”系统将自动依次递增编号；
2.对于有默认约束的字段，可以插入“DEFAULT”表示使用默认值；
3.列值可传入数值、表达式或函数，如密码可以用md5()函数进行加密（如md5('123')）；
4.可同时插入多条记录，多条记录括号间用逗号“,”隔开
```

```mysql
# 在特定列插入数据
INSERT INTO TABLE tb_name (col_name) VALUES (),(),();

-------------------------------------------------------
# 从其他表格复制数据
INSERT INTO TABLE tb_name (col_name)
SELECT * FROM other_tb_name WHERE ... ;
```

### 删除数据

```mysql
DELETE FROM tb_name WHERE col_name='exist_value';

# 示例
DELETE FROM students WHERE userid = 2;
--->
+─────+─────────+───────────+──────+─────────+────────+
| id  | userid  | username  | age  | gender  | class  |
+─────+─────────+───────────+──────+─────────+────────+
| 1   | 1       | jack      | 10   | 0       | 1      |
+─────+─────────+───────────+──────+─────────+────────+
1.不添加WHERE则删除全部记录
2.删除单条记录后再插入，插入的记录中id编号将从最大值往上加，而不是填补删除的。

# 示例
INSERT INTO  students VALUES (DEFAULT,1,"tom",11,'0',3);
--->
+─────+─────────+───────────+──────+─────────+────────+
| id  | userid  | username  | age  | gender  | class  |
+─────+─────────+───────────+──────+─────────+────────+
| 1   | 1       | jack      | 10   | 0       | 1      |
| 3   | 1       | tom       | 11   | 0       | 3      |
+─────+─────────+───────────+──────+─────────+────────+
```

### 修改数据

```mysql
# 修改列所有数据
UPDATE tb_name SET col_name='new_value'

# 示例
UPDATE students SET age = 11;   # 谨慎使用
--->
+─────+─────────+───────────+──────+─────────+────────+
| id  | userid  | username  | age  | gender  | class  |
+─────+─────────+───────────+──────+─────────+────────+
| 1   | 1       | jack      | 11   | 0       | 1      |
| 3   | 1       | tom       | 11   | 0       | 3      |
+─────+─────────+───────────+──────+─────────+────────+

---------------------------------------------------------------
# 修改列特定行的数据
UPDATE tb_name SET col1_name='new1_value',col2_name='new2_value'
WHERE col_name='exist_value';

# 示例
UPDATE students SET age = 9,userid = 22
WHERE id = 3;
--->
+─────+─────────+───────────+──────+─────────+────────+
| id  | userid  | username  | age  | gender  | class  |
+─────+─────────+───────────+──────+─────────+────────+
| 1   | 1       | jack      | 11   | 0       | 1      |
| 3   | 22      | tom       | 9    | 0       | 3      |
+─────+─────────+───────────+──────+─────────+────────+
```

### 查询数据

```mysql
# 标准语法
SELECT DISTINCT col_name FROM tb_name
WHERE ...
GROUP BY ... HAVING ...
ORDER BY ... ASC/DESC
LIMIT ... OFFSET ... ;
```

```mysql
# 示例数据：mysql自带表city
+─────+─────────────────+──────────────+────────────────+─────────────+
| ID  | Name            | CountryCode  | District       | Population  |
+─────+─────────────────+──────────────+────────────────+─────────────+
| 1   | Kabul           | AFG          | Kabol          | 1780000     |
| 2   | Qandahar        | AFG          | Qandahar       | 237500      |
| 3   | Herat           | AFG          | Herat          | 186800      |
| 4   | Mazar-e-Sharif  | AFG          | Balkh          | 127800      |
| 5   | Amsterdam       | NLD          | Noord-Holland  | 731200      |
| 6   | Rotterdam       | NLD          | Zuid-Holland   | 593321      |
| 7   | Haag            | NLD          | Zuid-Holland   | 440900      |
| 8   | Utrecht         | NLD          | Utrecht        | 234323      |
| 9   | Eindhoven       | NLD          | Noord-Brabant  | 201843      |
| 10  | Tilburg         | NLD          | Noord-Brabant  | 193238      |
```

```mysql
# LIMIT用法
SELECT * FROM city LIMIT 2,3;   #从第三行开始取数据，连取3行
等同于
SELECT * FROM city LIMIT 3 OFFSET 2;
--->
+─────+─────────────────+──────────────+────────────────+─────────────+
| ID  | Name            | CountryCode  | District       | Population  |
+─────+─────────────────+──────────────+────────────────+─────────────+
| 3   | Herat           | AFG          | Herat          | 186800      |
| 4   | Mazar-e-Sharif  | AFG          | Balkh          | 127800      |
| 5   | Amsterdam       | NLD          | Noord-Holland  | 731200      |
+─────+─────────────────+──────────────+────────────────+─────────────+
```

```mysql
# GROUP BY / HAVING 用法
SELECT 
    Population, COUNT(*)
FROM
    city
GROUP BY Population
HAVING COUNT(*) > 1
ORDER BY COUNT(*) DESC;
--->
+─────────────+───────────+
| Population  | count(*)  |
+─────────────+───────────+
| 90000       | 12        |
| 101000      | 6         |
| 130000      | 4         |
| 112000      | 4         |
| 106000      | 4         |
+─────────────+───────────+
```

```mysql
# 字符连接
# concat用法
SELECT concat(CountryCode,'-',Name) FROM city;
--->
+───────────────────────────────+
| concat(CountryCode,'-',Name)  |
+───────────────────────────────+
| AFG-Kabul                     |
| AFG-Qandahar                  |
| AFG-Herat                     |
+───────────────────────────────+

-------------------------------------------------
# 组内字符连接
# GROUP_CONCAT()
SELECT CountryCode, GROUP_CONCAT(District) FROM city
GROUP BY CountryCode;
--->
+──────────────+───────────────────────────────────────────────────────────────────────+
| CountryCode  | group_concat(District)                                                |
+──────────────+───────────────────────────────────────────────────────────────────────+
| ABW          | Â–                                                                    |
| AFG          | Kabol,Qandahar,Herat,Balkh                                            |
| AGO          | Luanda,Huambo,Benguela,Benguela,Namibe                                |
| AIA          | Â–,Â–                                                                 |
| ALB          | Tirana                                                                |
+──────────────+───────────────────────────────────────────────────────────────────────+

# 组内字符连接，去掉重复值
SELECT CountryCode, GROUP_CONCAT(DISTINCT District) FROM city
GROUP BY CountryCode;
--->
+──────────────+────────────────────────────────────────────────────────────────+
| CountryCode  | group_concat(DISTINCT District)                                |
+──────────────+────────────────────────────────────────────────────────────────+
| ABW          | Â–                                                             |
| AFG          | Balkh,Herat,Kabol,Qandahar                                     |
| AGO          | Benguela,Huambo,Luanda,Namibe                                  |
| AIA          | Â–                                                             |
| ALB          | Tirana                                                         |
+──────────────+────────────────────────────────────────────────────────────────+
```

```mysql
# Like用法
SELECT * FROM city WHERE District LIKE "%java%"
--->
+─────+───────────+──────────────+───────────+─────────────+
| ID  | Name      | CountryCode  | District  | Population  |
+─────+───────────+──────────────+───────────+─────────────+
| 940 | Surabaya  | IDN          | East Java | 2663820     |
| 941 | Bandung   | IDN          | West Java | 2429000     |
| 944 | Tangerang | IDN          | West Java | 1198300     |
+─────+───────────+──────────────+───────────+─────────────+
1.[NOT]LIKE  模糊匹配
2.(%)：代表任意个字符，0个或多个
3.(_)：代表任意一个字符，只有一个
```

```mysql
# COALESCE用法：返回列表中第一个非空值
SELECT COALESCE(NULL, NULL, NULL, 'W3Schools.com', NULL, 'Example.com');
--->
+───────────────────────────────────────────────────────────────────+
| COALESCE(NULL, NULL, NULL, 'W3Schools.com', NULL, 'Example.com')  |
+───────────────────────────────────────────────────────────────────+
| W3Schools.com                                                     |
+───────────────────────────────────────────────────────────────────+
```

```mysql
# CASE ... WHEN ... ELSE ... END 用法
SELECT Name,Population,
    (CASE
    WHEN (Population < 100000) THEN 'small'
    WHEN (Population BETWEEN 100000 AND 1000000) THEN 'middle'
    WHEN (Population BETWEEN 1000000 AND 5000000) THEN 'big'
    ELSE 'super' END ) AS x
FROM city;
--->
+─────────────────────────────────+─────────────+────────+
| Name                            | Population  | x      |
+─────────────────────────────────+─────────────+────────+
| SÃ£o Paulo                      | 9968485     | super  |
| Jakarta                         | 9604900     | super  |
| London                          | 7285000     | super  |
| Fortaleza                       | 2097757     | big    |
| Guayaquil                       | 2070040     | big    |
| Slough                          | 112000      | middle |
| Sohumi                          | 111700      | middle |
| The Valley                      | 595         | small  |
+─────────────────────────────────+─────────────+────────+
1.BETWEEN ... AND ... 包含边界
```

```mysql
# IN用法
SELECT Name,District,District IN ('Qandahar','Zuid-Holland') FROM city;
--->
+─────────────────+────────────────+──────────────────────────────────────────+
| Name            | District       | District IN ('Qandahar','Zuid-Holland')  |
+─────────────────+────────────────+──────────────────────────────────────────+
| Kabul           | Kabol          | 0                                        |
| Qandahar        | Qandahar       | 1                                        |
| Herat           | Herat          | 0                                        |
| Mazar-e-Sharif  | Balkh          | 0                                        |
| Amsterdam       | Noord-Holland  | 0                                        |
| Rotterdam       | Zuid-Holland   | 1                                        |
| Haag            | Zuid-Holland   | 1                                        |
+─────────────────+────────────────+──────────────────────────────────────────+
```

```mysql
# SOME/ANY/ALL用法
>>> 该国家所有城市人口都超过100万的国家有哪些？
SELECT Name,CountryCode,Population FROM city x
WHERE 100000 >= ANY (SELECT Population FROM city y WHERE x.CountryCode = y.CountryCode);
--->
+──────────────────────────+──────────────+─────────────+
| Name                     | CountryCode  | Population  |
+──────────────────────────+──────────────+─────────────+
| Conakry                  | GIN          | 1090610     |
| Kowloon and New Kowloon  | HKG          | 1987996     |
| Victoria                 | HKG          | 1312637     |
| Singapore                | SGP          | 4017733     |
| Montevideo               | URY          | 1236000     |
+──────────────────────────+──────────────+─────────────+
```

![anysomeall](anysomeall.png)

```mysql
# IFNULL用法：如果表达式为空则返回特定值
>>> 返回人口第二高的城市
SELECT IFNULL(
    (SELECT Population FROM city ORDER BY Population DESC LIMIT 1 OFFSET 1),NULL) AS SecondHighest
--->
+────────────────+
| SecondHighest  |
+────────────────+
| 9981619        |
+────────────────+
```






```mysql

```


#### 连接查询

![数据连接表](数据连接表.png)

```mysql
# 表与常数连接
SELECT * FROM city,(SELECT 1) AS n;
--->
+─────+─────────────────+──────────────+────────────────+─────────────+────+
| ID  | Name            | CountryCode  | District       | Population  | n  |
+─────+─────────────────+──────────────+────────────────+─────────────+────+
| 1   | Kabul           | AFG          | Kabol          | 1780000     | 1  |
| 2   | Qandahar        | AFG          | Qandahar       | 237500      | 1  |
| 3   | Herat           | AFG          | Herat          | 186800      | 1  |
| 4   | Mazar-e-Sharif  | AFG          | Balkh          | 127800      | 1  |
| 5   | Amsterdam       | NLD          | Noord-Holland  | 731200      | 1  |
+─────+─────────────────+──────────────+────────────────+─────────────+────+
```

#### 时间查询

```mysql
# 查询时间差

# TIMESTAMPDIFF()语法：返回给定的单位时间距离
TIMESTAMPDIFF(unit,date_time_1,date_time_2);
#unit:SECOND, MINUTE, HOUR, DAY, WEEK, MONTH, QUARTER, or YEAR

# 示例
SELECT TIMESTAMPDIFF(MONTH,'2009-05-18','2009-07-29');
--->
+───────────────────────────────────────────────────+
| # TIMESTAMPDIFF(MONTH,'2009-05-18','2009-07-29')  |
+───────────────────────────────────────────────────+
| 2                                                 |
+───────────────────────────────────────────────────+

----------------------------------------------------------------
# DATEDIFF()语法：返回日期差
DATEDIFF(date_time_1,date_time_2);

# 示例
SELECT DATEDIFF('2008-05-17 11:31:31','2008-04-28');
--->
+───────────────────────────────────────────────+
| DATEDIFF('2008-05-17 11:31:31','2008-04-28')  |
+───────────────────────────────────────────────+
| 19                                            |
+───────────────────────────────────────────────+

----------------------------------------------------------------
# TIMEDIFF()语法：返回日期差
TIMEDIFF(date_time_1,date_time_2);

# 示例
SELECT TIMEDIFF('2009-05-18 15:45:57.005678','2009-05-18 13:40:50.005670');
--->
+──────────────────────────────────────────────────────────────────────+
| TIMEDIFF('2009-05-18 15:45:57.005678','2009-05-18 13:40:50.005670')  |
+──────────────────────────────────────────────────────────────────────+
| 02:05:07.000008                                                      |
+──────────────────────────────────────────────────────────────────────+
```

```mysql
# 查询当前日期
SELECT CURDATE();
--->
+--------------+
| CURRENT_DATE |
+--------------+
| 2015-04-13   |
+--------------+

# 查询当前时间
SELECT CURTIME();
--->
+--------------+
| CURRENT_TIME |
+--------------+
| 11:35:45     |
+--------------+

# 查询当前日期 + 时间
SELECT CURRENT_TIMESTAMP; / SELECT NOW();
+---------------------+
| CURRENT_TIMESTAMP   |
+---------------------+
| 2015-04-13 11:42:41 |
+---------------------+
```

```mysql
# 提取日期：DATE()
SELECT DATE('2008-05-17 11:31:31')
--->
+---------------+
| required_DATE |
+---------------+
| 2008-05-17    |
+---------------+

# 提取年份：YEAR()
SELECT YEAR('2009-05-19');
--->
+--------------------+
| YEAR('2009-05-19') |
+--------------------+
|               2009 |
+--------------------+

# 提取月份：MONTH()
SELECT MONTH('2009-05-18');
--->
+---------------------+
| MONTH('2009-05-18') |
+---------------------+
|                   5 |
+---------------------+

# 提取日期：DAY()
SELECT DAY('2008-05-15');
--->
+-------------------+
| DAY('2008-05-15') |
+-------------------+
|                15 |
+-------------------+
```

```mysql
# 日期/时间 加减

# 单次只能加减日期或时间
# TIMESTAMPADD()
TIMESTAMPADD(unit,interval,datetime_expr);
#unit:SECOND, MINUTE, HOUR, DAY, WEEK, MONTH, QUARTER, or YEAR

# 示例
SELECT TIMESTAMPADD(MONTH,2,'2009-05-18');
--->
+------------------------------------+
| TIMESTAMPADD(MONTH,2,'2009-05-18') |
+------------------------------------+
| 2009-07-18                         |
+------------------------------------+

# 同时可以加减日期和时间
# ADDTIME(expr1,expr2)

# 示例
SELECT ADDTIME('2008-05-15 13:20:32.50','2 1:39:27.50');
--->
+----------------------------+
| required_datetime          |
+----------------------------+
| 2008-05-17 15:00:00.000000 |
+----------------------------+

# 只能加减日期
# ADDDATE(date, INTERVAL expr unit) / ADDDATE(expr,days)

# 示例
SELECT ADDDATE('2008-05-15', INTERVAL 10 DAY);
--->
+---------------+
| required_date |
+---------------+
| 2008-05-25    |
+---------------+
```

```mysql
# 日期格式
# DATE_FORMAT(date,format)

# 示例
SELECT DATE_FORMAT('2008-05-15 22:23:00', '%W %D %M %Y');
--->
+---------------------------------------------------+
| DATE_FORMAT('2008-05-15 22:23:00', '%W %D %M %Y') |
+---------------------------------------------------+
| Thursday 15th May 2008                            |
+---------------------------------------------------+

# 格式表
+───────+──────────────────────────────────────────────────────────────────────────────────────────────────+
| Name  | Description                                                                                      |
+───────+──────────────────────────────────────────────────────────────────────────────────────────────────+
| %a    | Abbreviated weekday name (Sun..Sat)                                                              |
| %b    | Abbreviated month name (Jan..Dec)                                                                |
| %ac   | Month, numeric (0..12)                                                                           |
| %D    | Day of the month with English suffix (0th, 1st, 2nd, 3rd, …)                                     |
| %d    | Day of the month, numeric (00..31)                                                               |
| %e    | Day of the month, numeric (0..31)                                                                |
| %f    | Microseconds (000000..999999)                                                                    |
| %H    | Hour (00..23)                                                                                    |
| %h    | Hour (01..12)                                                                                    |
| %I    | Hour (01..12)                                                                                    |
| %i    | Minutes, numeric (00..59)                                                                        |
| %j    | Day of year (001..366)                                                                           |
| %k    | Hour (0..23)                                                                                     |
| %l    | Hour (1..12)                                                                                     |
| %M    | Month name (January..December)                                                                   |
| %m    | Month, numeric (00..12)                                                                          |
| %p    | AM or PM                                                                                         |
| %r    | Time, 12-hour (hh:mm:ss followed by AM or PM)                                                    |
| %S    | Seconds (00..59)                                                                                 |
| %s    | Seconds (00..59)                                                                                 |
| %T    | Time, 24-hour (hh:mm:ss)                                                                         |
| %U    | Week (00..53), where Sunday is the first day of the week                                         |
| %u    | Week (00..53), where Monday is the first day of the week                                         |
| %V    | Week (01..53), where Sunday is the first day of the week; used with %X                           |
| %v    | Week (01..53), where Monday is the first day of the week; used with %x                           |
| %W    | Weekday name (Sunday..Saturday)                                                                  |
| %w    | Day of the week (0=Sunday..6=Saturday)                                                           |
| %X    | Year for the week where Sunday is the first day of the week, numeric, four digits; used with %V  |
| %x    | Year for the week, where Monday is the first day of the week, numeric, four digits; used with %v |
| %Y    | Year, numeric, four digits                                                                       |
| %y    | Year, numeric (two digits)                                                                       |
| %%    | A literal “%” character                                                                          |
| %x    | x, for any “x” not listed above                                                                  |
+───────+──────────────────────────────────────────────────────────────────────────────────────────────────+

```
