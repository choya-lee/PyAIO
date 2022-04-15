## 什么是PyMySQL

PyMySQL是从Python连接到MySQL数据库服务器的接口。 它实现了Python数据库[API](https://so.csdn.net/so/search?q=API&spm=1001.2101.3001.7020) v2.0，并包含一个纯Python的MySQL客户端库。 PyMySQL的目标是成为MySQLdb的替代品。

PyMySQL参考文档：http://pymysql.readthedocs.io/

## 安装PyMySQL

在使用PyMySQL之前，请确保您的机器上安装了PyMySQL。只需在Python脚本中输入以下内容即可执行它 -

```python
import pymysql
```

## 数据库连接

在连接到MySQL数据库之前，请确保以下几点：

- 已经创建了一个数据库：`test`。
- 已经在`test`中创建了一个表:`employee`。
- `employee`表格包含:`fist_name`，`last_name`，`age`，`sex`和`income`字段。
- MySQL用户“root”和密码“123456”可以访问：`test`。
- Python模块PyMySQL已正确安装在您的计算机上。
- 已经通过[MySQL教程](http://www.yiibai.com/mysql)了解MySQL基础知识。

创建表`employee`的语句为：

```sql
 CREATE TABLE `employee` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `first_name` char(20) NOT NULL,
  `last_name` char(20) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `sex` char(1) DEFAULT NULL,
  `income` float DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

实例

以下是Python通过PyMySQL模块接口连接MySQL数据库“`test`”的示例 -

> 注意：在 Windows 系统上，`import PyMySQL` 和 `import pymysql` 有区别。

```python
import pymysql

# Open database connection
db = pymysql.connect("localhost","root","123456","test" )

# prepare a cursor object using cursor() method
cursor = db.cursor()

# execute SQL query using execute() method.
cursor.execute("SELECT VERSION()")

# Fetch a single row using fetchone() method.
data = cursor.fetchone()

print ("Database version : %s " % data)

# disconnect from server
db.close()
```

运行此脚本时，会产生以下结果 -

```shell
Database version : 5.7.14-log
```

如果使用数据源建立连接，则会返回连接对象并将其保存到`db`中以供进一步使用，否则将`db`设置为`None`。 接下来，`db`对象用于创建一个游标对象，用于执行SQL查询。 最后，在结果打印出来之前，它确保数据库连接关闭并释放资源。

## 创建数据库表

建立数据库连接后，可以使用创建的游标的`execute`方法将数据库表或记录创建到数据库表中。

示例

下面演示如何在数据库：`test`中创建一张数据库表：`employee` 

```python
import pymysql

# Open database connection
db = pymysql.connect("localhost","root","123456","test" )

# prepare a cursor object using cursor() method
cursor = db.cursor()

# Drop table if it already exist using execute() method.
cursor.execute("DROP TABLE IF EXISTS employee")

# Create table as per requirement
sql = """CREATE TABLE `employee` (
  `id` int(10) NOT NULL AUTO_INCREMENT,
  `first_name` char(20) NOT NULL,
  `last_name` char(20) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `sex` char(1) DEFAULT NULL,
  `income` float DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;"""

cursor.execute(sql)

print("Created table Successfull.")

# disconnect from server
db.close()
```

运行此脚本时，会产生以下结果 -

```shell
Created table Successfull.
```

## 插入操作

当要将记录创建到数据库表中时，需要执行`INSERT`操作。

示例

以下示例执行SQL的`INSERT`语句以在`EMPLOYEE`表中创建一条(多条)记录 -

```python
import pymysql

# Open database connection
db = pymysql.connect("localhost","root","123456","test" )

# prepare a cursor object using cursor() method
cursor = db.cursor()

# Prepare SQL query to INSERT a record into the database.
sql = """INSERT INTO EMPLOYEE(FIRST_NAME,
   LAST_NAME, AGE, SEX, INCOME)
   VALUES ('Mac', 'Su', 20, 'M', 5000)"""

try:
   # Execute the SQL command
   cursor.execute(sql)
   # Commit your changes in the database
   db.commit()
except:
   # Rollback in case there is any error
   db.rollback()

## 再次插入一条记录
# Prepare SQL query to INSERT a record into the database.
sql = """INSERT INTO EMPLOYEE(FIRST_NAME,
   LAST_NAME, AGE, SEX, INCOME)
   VALUES ('Kobe', 'Bryant', 40, 'M', 8000)"""

try:
   # Execute the SQL command
   cursor.execute(sql)
   # Commit your changes in the database
   db.commit()
except:
   # Rollback in case there is any error
   db.rollback()

print (sql)
print('Yes, Insert Successfull.')

# disconnect from server
db.close()
```

运行此脚本时，会产生以下结果 -

```shell
Yes, Insert Successfull.
```

上述插入示例可以写成如下动态创建SQL查询 -

```python
import pymysql
 
# Open database connection
db = pymysql.connect("localhost","root","123456","test" )
 
# prepare a cursor object using cursor() method
cursor = db.cursor()
 
# Prepare SQL query to INSERT a record into the database.
sql = "INSERT INTO EMPLOYEE(FIRST_NAME, \
   LAST_NAME, AGE, SEX, INCOME) \
   VALUES ('%s', '%s', '%d', '%c', '%d' )" % \
   ('Max', 'Su', 25, 'F', 2800)
try:
   # Execute the SQL command
   cursor.execute(sql)
   # Commit your changes in the database
   db.commit()
except:
   # Rollback in case there is any error
   db.rollback()
 
# disconnect from server
db.close()
```

Python

示例

以下代码段是另一种执行方式，可以直接传递参数 -

```python
 
..................................
user_id = "test123"
password = "password"
 
con.execute('insert into Login values("%s", "%s")' % \
             (user_id, password))
..................................
```

## 读取操作

任何数据库上的读操作表示要从数据库中读取获取一些有用的信息。

在建立数据库连接后，就可以对此数据库进行查询了。 可以使用`fetchone()`方法获取单条记录或`fetchall()`方法从数据库表中获取多个值。

- `fetchone()` - 它获取查询结果集的下一行。 结果集是当使用游标对象来查询表时返回的对象。
- `fetchall()` - 它获取结果集中的所有行。 如果已经从结果集中提取了一些行，则从结果集中检索剩余的行。
- `rowcount` - 这是一个只读属性，并返回受`execute()`方法影响的行数。

示例

以下过程查询`EMPLOYEE`表中所有记录的工资超过`1000`员工记录信息 -

```python
import pymysql
 
# Open database connection
db = pymysql.connect("localhost","root","123456","test" )
 
# prepare a cursor object using cursor() method
cursor = db.cursor()
# 按字典返回 
# cursor = db.cursor(pymysql.cursors.DictCursor)
 
# Prepare SQL query to select a record from the table.
sql = "SELECT * FROM EMPLOYEE \
       WHERE INCOME > %d" % (1000)
#print (sql)
try:
   # Execute the SQL command
   cursor.execute(sql)
   # Fetch all the rows in a list of lists.
   results = cursor.fetchall()
   for row in results:
      #print (row)
      fname = row[1]
      lname = row[2]
      age = row[3]
      sex = row[4]
      income = row[5]
      # Now print fetched result
      print ("name = %s %s,age = %s,sex = %s,income = %s" % \
             (fname, lname, age, sex, income ))
except:
   import traceback
   traceback.print_exc()
 
   print ("Error: unable to fetch data")
 
# disconnect from server
db.close()
```

```shell
name = Mac Su,age = 20,sex = M,income = 5000.0
name = Kobe Bryant,age = 40,sex = M,income = 8000.0
```

## 更新操作

UPDATE语句可对任何数据库中的数据进行更新操作，它可用于更新数据库中已有的一个或多个记录。

以下程序将所有`SEX`字段的值为“`M`”的记录的年龄(`age`字段)更新为增加一年。

```python
import pymysql
 
# Open database connection
db = pymysql.connect("localhost","root","123456","test" )
 
# prepare a cursor object using cursor() method
#cursor = db.cursor()
cursor = db.cursor(pymysql.cursors.DictCursor)
# prepare a cursor object using cursor() method
cursor = db.cursor()
 
# Prepare SQL query to UPDATE required records
sql = "UPDATE EMPLOYEE SET AGE = AGE + 1 \
                          WHERE SEX = '%c'" % ('M')
try:
   # Execute the SQL command
   cursor.execute(sql)
   # Commit your changes in the database
   db.commit()
except:
   # Rollback in case there is any error
   db.rollback()
 
# disconnect from server
db.close()
```

## 删除操作

当要从数据库中删除一些记录时，那么可以执行`DELETE`操作。 以下是删除`EMPLOYEE`中`AGE`超过`40`的所有记录的程序 -

```python
import pymysql
 
# Open database connection
db = pymysql.connect("localhost","root","123456","test" )
 
# prepare a cursor object using cursor() method
cursor = db.cursor()
 
# Prepare SQL query to DELETE required records
sql = "DELETE FROM EMPLOYEE WHERE AGE > '%d'" % (40)
try:
   # Execute the SQL command
   cursor.execute(sql)
   # Commit your changes in the database
   db.commit()
except:
   # Rollback in case there is any error
   db.rollback()
 
# disconnect from server
db.close()
```

## 执行事务

事务是确保数据一致性的一种机制。事务具有以下四个属性 -

- 原子性 - 事务要么完成，要么完全没有发生。
- 一致性 - 事务必须以一致的状态开始，并使系统保持一致状态。
- 隔离性 - 事务的中间结果在当前事务外部不可见。
- 持久性 - 当提交了一个事务，即使系统出现故障，效果也是持久的。

Python DB API 2.0提供了两种提交或回滚事务的方法。

示例

已经知道如何执行事务。 这是一个类似的例子 

```python
 # Prepare SQL query to DELETE required records
sql = "DELETE FROM EMPLOYEE WHERE AGE > '%d'" % (20)
try:
   # Execute the SQL command
   cursor.execute(sql)
   # Commit your changes in the database
   db.commit()
except:
   # Rollback in case there is any error
   db.rollback()
```

### COMMIT操作

提交是一种操作，它向数据库发出信号以完成更改，并且在此操作之后，不会更改任何更改。

下面是一个简单的例子演示如何调用`commit()`方法。

```python
db.commit()
```

### 回滚操作

如果您对一个或多个更改不满意，并且要完全还原这些更改，请使用`rollback()`方法。

下面是一个简单的例子演示如何调用`rollback()`方法。

```python
db.rollback()
```

## 断开数据库连接

要断开数据库连接，请使用`close()`方法。

```python
db.close()
```

如果用户使用`close()`方法关闭与数据库的连接，则任何未完成的事务都将被数据库回滚。 但是，您的应用程序不会依赖于数据级别的实现细节，而是明确地调用提交或回滚。

## 处理错误

错误有很多来源。一些示例是执行的SQL语句中的语法错误，连接失败或为已取消或已完成语句句柄调用`fetch`方法等等。

DB API定义了每个数据库模块中必须存在的许多错误。下表列出了这些异常和错误 -

| 编号 |       异常        |                             描述                             |
| :--: | :---------------: | :----------------------------------------------------------: |
|  1   |      Warning      |           用于非致命问题，是StandardError的子类。            |
|  2   |       Error       |             错误的基类，是StandardError的子类。              |
|  3   |  InterfaceError   |  用于数据库模块中的错误，但不是数据库本身，是Error的子类。   |
|  4   |   DatabaseError   |             用于数据库中的错误，是Error的子类。              |
|  5   |     DataError     |            DatabaseError的子类引用数据中的错误。             |
|  6   | OperationalError  | DatabaseError的子类，涉及如丢失与数据库的连接等错误。 这些错误通常不在Python脚本程序的控制之内。 |
|  7   |  IntegrityError   | DatabaseError 子类错误，可能会损害关系完整性，例如唯一性约束和外键。 |
|  8   |   InternalError   | DatabaseError的子类，指的是数据库模块内部的错误，例如游标不再活动。 |
|  9   | ProgrammingError  |  DatabaseError的子类，它引用错误，如错误的表名和其他安全。   |
|  10  | NotSupportedError |       DatabaseError的子类，用于尝试调用不支持的功能。        |

Python脚本应该处理这些错误，但在使用任何上述异常之前，请确保您的PyMySQL支持该异常。 可以通过阅读DB API 2.0规范获得更多关于它们的信息。