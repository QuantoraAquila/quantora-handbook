# SQL 学习笔记（Python程序员版）

---

# 1. 什么是 SQL

SQL（Structured Query Language，结构化查询语言）是一种用于操作关系型数据库的语言。

常见数据库：

- MySQL
- PostgreSQL
- SQLite
- Oracle
- SQL Server

SQL ≠ 编程语言

Python属于：

- 通用编程语言
- 有变量
- 有循环
- 有函数

SQL属于：

- 声明式语言（Declarative Language）
- 告诉数据库"要什么"
- 不关心具体怎么实现

例如：

Python：

```python
result = []
for user in users:
    if user.age > 18:
        result.append(user)
```

SQL：

```sql
SELECT *
FROM users
WHERE age > 18;
```

SQL只描述：

> 我要年龄大于18岁的用户

至于如何查找，由数据库自己决定。

---

# 2. 数据库核心概念

## 表（Table）

类似Python中的二维列表。

用户表：

| id | name | age |
|----|------|-----|
| 1 | Tom | 18 |
| 2 | Jack | 20 |

SQL：

```sql
SELECT * FROM users;
```

---

## 行（Row）

一条记录：

```text
1 | Tom | 18
```

类似：

```python
{
    "id":1,
    "name":"Tom",
    "age":18
}
```

---

## 列（Column）

字段：

```text
id
name
age
```

类似：

```python
class User:
    id
    name
    age
```

---

# 3. SQL语法规则

## 规则1：关键字不区分大小写

下面完全等价：

```sql
select * from users;
```

```sql
SELECT * FROM users;
```

推荐：

```sql
SELECT * FROM users;
```

关键字全部大写。

---

## 规则2：字符串使用单引号

正确：

```sql
SELECT *
FROM users
WHERE name = 'Tom';
```

错误：

```sql
WHERE name = Tom
```

---

## 规则3：语句以分号结束

```sql
SELECT * FROM users;
```

---

## 规则4：注释

单行：

```sql
-- 查询所有用户
SELECT * FROM users;
```

多行：

```sql
/*
查询所有用户
*/
SELECT * FROM users;
```

---

# 4. SELECT 查询

最常用语句。

## 查询全部

```sql
SELECT *
FROM users;
```

Python类比：

```python
for user in users:
    print(user)
```

---

## 查询指定列

```sql
SELECT name, age
FROM users;
```

Python：

```python
for user in users:
    print(user["name"], user["age"])
```

---

# 5. WHERE 条件查询

## 等于

```sql
SELECT *
FROM users
WHERE age = 18;
```

Python：

```python
for user in users:
    if user["age"] == 18:
        print(user)
```

---

## 大于

```sql
WHERE age > 18
```

---

## 小于

```sql
WHERE age < 18
```

---

## 不等于

```sql
WHERE age <> 18
```

或者：

```sql
WHERE age != 18
```

---

# 6. 逻辑运算

## AND

```sql
SELECT *
FROM users
WHERE age > 18
AND gender = 'male';
```

Python：

```python
if age > 18 and gender == "male":
    pass
```

---

## OR

```sql
WHERE age > 18
OR score > 90
```

Python：

```python
if age > 18 or score > 90:
    pass
```

---

## NOT

```sql
WHERE NOT age = 18
```

Python：

```python
if not age == 18:
    pass
```

---

# 7. 排序 ORDER BY

## 升序 ASC

```sql
SELECT *
FROM users
ORDER BY age ASC;
```

Python：

```python
sorted(users, key=lambda x: x["age"])
```

---

## 降序 DESC

```sql
SELECT *
FROM users
ORDER BY age DESC;
```

Python：

```python
sorted(
    users,
    key=lambda x: x["age"],
    reverse=True
)
```

---

# 8. LIMIT 限制结果

取前5条：

```sql
SELECT *
FROM users
LIMIT 5;
```

Python：

```python
users[:5]
```

---

# 9. 模糊查询 LIKE

## 包含

```sql
WHERE name LIKE '%Tom%'
```

Python：

```python
if "Tom" in name:
    pass
```

---

## 开头

```sql
WHERE name LIKE 'Tom%'
```

Python：

```python
name.startswith("Tom")
```

---

## 结尾

```sql
WHERE name LIKE '%Tom'
```

Python：

```python
name.endswith("Tom")
```

---

# 10. IN 查询

SQL：

```sql
SELECT *
FROM users
WHERE age IN (18,20,22);
```

Python：

```python
if age in [18,20,22]:
    pass
```

---

# 11. BETWEEN 查询

SQL：

```sql
SELECT *
FROM users
WHERE age BETWEEN 18 AND 30;
```

Python：

```python
18 <= age <= 30
```

---

# 12. NULL

NULL表示：

```text
未知
空值
没有数据
```

不是：

```text
0
''
False
```

---

查询NULL：

```sql
WHERE phone IS NULL
```

Python：

```python
phone is None
```

---

查询非NULL：

```sql
WHERE phone IS NOT NULL
```

Python：

```python
phone is not None
```

---

# 13. 聚合函数

用于统计。

---

## COUNT

统计数量：

```sql
SELECT COUNT(*)
FROM users;
```

Python：

```python
len(users)
```

---

## SUM

求和：

```sql
SELECT SUM(score)
FROM users;
```

Python：

```python
sum(scores)
```

---

## AVG

平均值：

```sql
SELECT AVG(score)
FROM users;
```

Python：

```python
sum(scores)/len(scores)
```

---

## MAX

最大值：

```sql
SELECT MAX(score)
FROM users;
```

Python：

```python
max(scores)
```

---

## MIN

最小值：

```sql
SELECT MIN(score)
FROM users;
```

Python：

```python
min(scores)
```

---

# 14. GROUP BY 分组

统计每个部门人数：

```sql
SELECT dept,
       COUNT(*)
FROM employees
GROUP BY dept;
```

Python：

```python
result = {}

for emp in employees:
    dept = emp["dept"]

    if dept not in result:
        result[dept] = 0

    result[dept] += 1
```

---

# 15. HAVING

过滤分组结果。

SQL：

```sql
SELECT dept,
       COUNT(*)
FROM employees
GROUP BY dept
HAVING COUNT(*) > 10;
```

执行顺序：

```text
GROUP BY
↓
HAVING
```

注意：

```sql
WHERE
```

过滤行。

```sql
HAVING
```

过滤组。

---

# 16. INSERT 插入数据

```sql
INSERT INTO users(name, age)
VALUES ('Tom', 18);
```

Python：

```python
users.append(
    {
        "name":"Tom",
        "age":18
    }
)
```

---

# 17. UPDATE 更新数据

SQL：

```sql
UPDATE users
SET age = 20
WHERE id = 1;
```

Python：

```python
user["age"] = 20
```

---

# 18. DELETE 删除数据

SQL：

```sql
DELETE FROM users
WHERE id = 1;
```

Python：

```python
users.remove(user)
```

---

# 19. CREATE TABLE 建表

```sql
CREATE TABLE users (
    id INT,
    name VARCHAR(100),
    age INT
);
```

类似Python：

```python
class User:
    id: int
    name: str
    age: int
```

---

# 20. 常见数据类型

| SQL | Python |
|-------|--------|
| INT | int |
| BIGINT | int |
| FLOAT | float |
| DECIMAL | Decimal |
| VARCHAR | str |
| TEXT | str |
| DATE | date |
| DATETIME | datetime |
| BOOLEAN | bool |

---

# 21. 主键 Primary Key

主键特点：

- 唯一
- 不能重复
- 不能为空

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);
```

Python理解：

```python
user_id
```

作为唯一标识。

---

# 22. 外键 Foreign Key

用户表：

```text
users
```

订单表：

```text
orders
```

关系：

```text
users.id
    ↓
orders.user_id
```

SQL：

```sql
FOREIGN KEY (user_id)
REFERENCES users(id)
```

---

# 23. JOIN 联表查询

最重要内容之一。

---

用户表：

```text
users
```

| id | name |
|----|------|
| 1 | Tom |

订单表：

```text
orders
```

| id | user_id | amount |
|----|----------|--------|
| 1 | 1 | 100 |

---

INNER JOIN

```sql
SELECT
    u.name,
    o.amount
FROM users u
INNER JOIN orders o
ON u.id = o.user_id;
```

Python：

```python
for user in users:
    for order in orders:

        if user["id"] == order["user_id"]:
            print(
                user["name"],
                order["amount"]
            )
```

---

# 24. SQL执行顺序

很多新手误以为：

```sql
SELECT
FROM
WHERE
```

执行顺序其实是：

```text
FROM

↓

JOIN

↓

WHERE

↓

GROUP BY

↓

HAVING

↓

SELECT

↓

ORDER BY

↓

LIMIT
```

必须牢记。

---

# 25. SQL学习路线

第一阶段（必须掌握）

- SELECT
- WHERE
- ORDER BY
- LIMIT
- INSERT
- UPDATE
- DELETE

---

第二阶段（核心）

- GROUP BY
- HAVING
- 聚合函数
- JOIN

---

第三阶段（进阶）

- 子查询
- 视图(View)
- 索引(Index)
- 事务(Transaction)
- 存储过程
- 窗口函数

---

# SQL与Python思维差异总结

| Python | SQL |
|----------|---------|
| for循环 | SELECT |
| if | WHERE |
| and | AND |
| or | OR |
| in | IN |
| None | NULL |
| len() | COUNT() |
| sum() | SUM() |
| max() | MAX() |
| min() | MIN() |
| sorted() | ORDER BY |
| append() | INSERT |
| 修改对象 | UPDATE |
| 删除对象 | DELETE |
| 字典分组 | GROUP BY |
| 双重循环关联 | JOIN |

---

# 一句话理解SQL

Python思维：

```python
告诉程序怎么做
```

SQL思维：

```sql
告诉数据库我要什么
```

SQL是面向数据的声明式语言，而Python是面向过程的编程语言。