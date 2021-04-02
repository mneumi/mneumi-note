## 对于数据库的定义

### 创建库

```sql
CREATE DATABASE IF NOT EXISTS 库名
DEFAULT CHARACTER SET utf8        # 设置默认字符集为utf8
COLLATE uf8_general_ci;           # 不区分大小写 case insensitive

CREATE DATABASE IF NOT EXISTS 库名
DEFAULT CHARACTER SET utf8        # 设置默认字符集为utf8
COLLATE uf8_general_cs;           # 区分大小写 case sensitive
```

### 修改库

直接修改文件夹的名称，不建议做

### 删除库

```sql
DROP DATABASE IF EXISTS 库名;
```

### 更改库的字符集

```sql
ALTER DATABASE 库名
CHARACTER SET gbk;
```

### 推荐的数据库编码

推荐字符集`utf8mb4`，因为`utf8mb4`用4个字节存储，可以存储 emoji，而`utf8`不能存储emoji

`utf8mb4`对应的排序规则：`utf8mb4_general_ci`和`utf8mb4_0900_ai_ci`



## 对于数据表的定义

### 创建表

```sql
CREATE TABLE 表名 (
  字段名 字段类型【(长度)】【约束】,
  字段名 字段类型【(长度)】【约束】,
  ...
  字段名 字段类型【(长度)】【约束】,
  字段名 字段类型【(长度)】【约束】
);
```

### 修改表

修改字段名/类型/约束

```sql
ALTER TABLE 表名
MODIFY COLUMN 字段名 类型【(长度)】【约束】;
```

添加新字段

```sql
ALTER TABLE 表名
ADD COLUMN 新字段名【(长度)】【约束】;
```

删除字段

```sql
ALTER TABLE 表名
DROP COLUMN 字段名;
```

修改表名

```sql
ALTER TABLE 表名
RENAME TO 新表名;
```

### 删除表

```sql
DROP TABLE IF EXISTS 表名;
```

### 表的复制

仅仅复制结构

```sql
CREATE TABLE 表名
LIKE 目标表名;
```

复制结构+数据

```sql
# 全部复制
CREATE TABLE 表名
SELECT * FROM 目标表名;

# 部分复制
CREATE TABLE 表名
SELECT 部分字段 FROM 目标表名
WHERE 筛选条件;
```



## 数据类型

### 整数类型

| 分类      | 占用字节 |
| --------- | -------- |
| Tinyint   | 1个字节  |
| Smallint  | 2个字节  |
| Mediumint | 3个字节  |
| Int       | 4个字节  |
| Bigint    | 8个字节  |

符号

* 默认有符号
* 设置无符号的方法，使用 UNSIGNED 关键字

```sql
CREATE TABLE t (
  t1 INT,
  t2 INT UNSIGNED
);
```

长度

* 长度表示最大显示宽度，当使用 zerofill 时，不够长度时，用0填空
* 如果不配合zerofill，则整型的长度没用

越界

* 如果插入的数值超出范围，则报 out of range 异常，并插入最接近的临界值

### 浮点数型

| 浮点数型 | 占用字节大小 |
| -------- | ------------ |
| Float    | 4个字节      |
| Double   | 8个字节      |

### 定点数型

DECIMAL(M,D)
* M：整数+小数的位数
* D：小数的位数
* M默认为10, D默认为0

### 字符

| 字符型    | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| char      | 固定长度，性能高，不能节约空间；长度可以是0到255之间的任何值；在被查询时，会删除后面的空格 |
| varchar   | 可变长度，性能低，能节约空间；长度可以指定为0到65535之间的值；在被查询时，不会删除后面的空格 |
| text      | 存储大的字符串类型                                           |
| binary    | 类似char，存储的是二进制                                     |
| varbinary | 类似varchar，存储的是二进制                                  |
| Blob      | 存储数据量大的二进制，比如视频，图片                         |

数据库一般不会直接存储 BINARY和VARBINARY，而是直接存储到文件系统中，数据库中只存储对应 url

### 日期

| 日期型    | 例子                                                         |
| --------- | ------------------------------------------------------------ |
| year      | 2018，支持的范围是 1901到2155，和 0000                       |
| date      | 2018-09-26，支持的范围是 '1000-01-01' 到 '9999-12-31         |
| time      | 17:11:37                                                     |
| datetime  | 2018-09-26 17:11:50【与时区无关，8个字节】，支持的范围是1000-01-01 00:00:00 到 9999-12-31 23:59:59 |
| timestamp | 20180926171211【与时区有关，4个字节】，支持的范围是 1970-01-01 00:00:01 到 2038-01-19 03:14:07 |

TIMESTAMP与DATATAME相比，推荐使用TIMESTAMP

### JSON类型

MySQL可以直接存储JSON，但不推荐使用，因为失去了关系

### 空间类型

存储经纬度，但不推荐使用，应该用三个字段存储 xyz，方便修改



## 数据约束

### 六大约束类型

| 约束     | 说明            |
| -------- | --------------- |
| 非空约束 | NOT NULL        |
| 默认约束 | DEFAULT 值      |
| 唯一约束 | UNIQUE          |
| 主键约束 | PRIMARY KEY     |
| 外键约束 | FOREIGN KEY     |
| 检查约束 | **MySQL不支持** |

### 列级约束与表级约束

列级约束：约束语法都不报错，但是外键约束没有效果

表级约束：支持主键约束，外键约束，唯一约束

```sql
CREATE TABLE 表名(
    字段名 字段类型 列级约束
    字段名 字段类型
    CONSTRAINT 表级约束名 表级约束类型(字段名)
)
```

列级约束例子

```sql
CREATE TABLE stuinfo (
    id      INT         PRIMARY KEY,                     # 主键约束
    stuName VARCHAR(20) NOT NULL,                        # 非空约束
    gender  CHAR(1)     DEFAULT 'm',                     # 默认约束
    seat    INT         UNIQUE,                          # 唯一约束
    major   INT         FOREIGN KEY REFERENCES major(id) # 外键约束，但是没有效果
);
```

表级约束例子

```sql
CREATE TABLE stuinfo (
    id      INT         
    stuName VARCHAR(20) NOT NULL,                        # 非空约束
    gender  CHAR(1)     DEFAULT 'm',                     # 默认约束
    seat    INT,         
    major   INT,         
    
    CONSTRAINT pk PRIMARY KEY(id), # 主键约束
    CONSTRAINT pk PRIMARY KEY(stuName, gender), # 联合主键
    CONSTRAINT uq UNIQUE(seat),    # 唯一约束
    CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id) # 外键约束
);
```

### 主键约束和唯一约束的区别

| 约束类型 | 保证唯一性 | 是否允许为空 | 允许多少个 | 是否允许组合 |
| -------- | ---------- | ------------ | ---------- | ------------ |
| 主键     | 保证       | 不允许       | 最多1个    | 允许         |
| 唯一     | 保证       | 允许         | 可以多个   | 允许         |

### 外键的OPTION

外键有两个option，分别是删除和更新

可选值为：RESTRICT（限制），CASCADE（级联更新）

如果不设置，则默认为 RESTRICT

```sql
ALTER TABLE `products` ADD FOREIGN KEY(brand_id) REFERENCES brand(id)
                                                 ON UPDATE CASCADE
                                                 ON DELETE RESTRICT;
```

### 如何修改外键的option？

不能直接改，分三步走

1. 查出表的定义，从而获取表的外键的名称

   ```sql
   SHOW CREATE `product`;
   ```

2. 从表中删除外键字段

   ```sql
   ALTER TABLE `product` DROP `product_foreign_12fds`;
   ```

3. 重新设置外键

   ```sql
   ALTER TABLE `products` ADD FOREIGN KEY(brand_id) REFERENCES brand(id)
                                                    ON UPDATE CASCADE
                                                    ON DELETE RESTRICT;
   ```

### 外键使用注意事项

外键关联的必须是Key，一般是 主键/唯一键

插入数据时，先插入主表，再插入从表

删除数据时，先删除从表，再删除主表

### 主键使用注意事项

不要把业务字段作为主键

不推荐使用联合主键

### 自动更新

除了6大约束外，还可以设置字段在数据更新时，更新为某个值

使用`ON UPDATE`

```sql
CREATE TABLE IF NOT EXISTS `moment` (
    id INT PRIMARY KEY AUTO_INCREMENT,
    updateAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY(userId) REFERENCES users(id)
);
```



## 数据库表三大范式

三大范式作用：数据库的三大范式是为了减少数据冗余

| 范式     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| 第一范式 | 表中的每一列的数据都是原子性的，即不可再分                   |
| 第二范式 | 满足第一范式的前提下，表中都要有主键，且每一列都与主键有关系 |
| 第三范式 | 满足第一范式和第二范式的前提下，表中每一列与主键具有直接关系，不能是间接关系 |



## 数据表设计技巧

### 一对一

例子：用户：name password phoneNumber + **身份证号**

方案1：全部信息保存到一张表中

方案2：分为两张表（避免单表的字段过多 或 避免敏感信息）

### 一对多

例子：一个品牌可以有多个产品

方案：使用外键（brand_id与brand.id）

### 多对多

例子：多个学生可以选择多种课程（一个学生选择多个课程，一个课程可以被多个学生选）

方案：多加一张关系表（关系表需要分别添加外键约束）

经验：A表先与关系表进行联合查询，把结果再与B表进行联合查询

案例：后台管理系统的权限控制：建立三张表，用户表，角色表，权限表