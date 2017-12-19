http://www.w3school.com.cn/sql/sql_distinct.asp

## SQL SELECT DISTINCT 语句(去重返回结果)

```
SELECT DISTINCT 列名称 FROM 表名称
```



## WHERE 子句

```
SELECT 列名称 FROM 表名称 WHERE 列 运算符 值
```

下面的运算符可在 WHERE 子句中使用：

| 操作符     | 描述           |
| ------- | ------------ |
| =       | 等于           |
| <>      | 不等于          |
| >       | 大于           |
| <       | 小于           |
| >=      | 大于等于         |
| <=      | 小于等于         |
| BETWEEN | 在某个范围内       |
| LIKE    | 搜索某种模式(模糊搜索) |

SQL 使用单引号来环绕*文本值*（大部分数据库系统也接受双引号）。如果是*数值*，请不要使用引号。



## ORDER BY 语句

以字母顺序显示公司名称（Company），并以数字顺序显示顺序号（OrderNumber）：

```
SELECT Company, OrderNumber FROM Orders ORDER BY Company, OrderNumber
```

结果：

| Company  | OrderNumber |
| -------- | ----------- |
| Apple    | 4698        |
| IBM      | 3532        |
| W3School | 2356        |
| W3School | 6953        |



以逆字母顺序显示公司名称：

```
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC
```

### 结果：

| Company  | OrderNumber |
| -------- | ----------- |
| W3School | 6953        |
| W3School | 2356        |
| IBM      | 3532        |
| Apple    | 4698        |



以逆字母顺序显示公司名称，并以数字顺序显示顺序号：

```
SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC, OrderNumber ASC
```

### 结果：

| Company  | OrderNumber |
| -------- | ----------- |
| W3School | 2356        |
| W3School | 6953        |
| IBM      | 3532        |
| Apple    | 4698        |



## INSERT INTO 语句

```
INSERT INTO 表名称 VALUES (值1, 值2,....)
```

```
INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
```

```
INSERT INTO Persons VALUES ('Gates', 'Bill', 'Xuanwumen 10', 'Beijing')
```

```
INSERT INTO Persons (LastName, Address) VALUES ('Wilson', 'Champs-Elysees')
```



## Update 语句

```
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
```

```
UPDATE Person SET FirstName = 'Fred' WHERE LastName = 'Wilson' 
```

```
UPDATE Person SET Address = 'Zhongshan 23', City = 'Nanjing' WHERE LastName = 'Wilson'
```



## DELETE 语句

```
DELETE FROM 表名称 WHERE 列名称 = 值
```

```
DELETE FROM Person WHERE LastName = 'Wilson' 
```

## 删除所有行

```
DELETE FROM table_name
```

```
DELETE * FROM table_name
```



## TOP 子句

TOP 子句用于规定要返回的记录的数目。对于拥有数千条记录的大型表来说，TOP 子句是非常有用的。

### MySQL 语法

```
SELECT column_name(s)
FROM table_name
LIMIT number
```

#### 例子

```
SELECT *
FROM Persons
LIMIT 5
```

### Oracle 语法

```
SELECT column_name(s)
FROM table_name
WHERE ROWNUM <= number
```

#### 例子

```
SELECT *
FROM Persons
WHERE ROWNUM <= 5
```

选取 50% 的记录。

```
SELECT TOP 50 PERCENT * FROM Persons
```























































































































































































