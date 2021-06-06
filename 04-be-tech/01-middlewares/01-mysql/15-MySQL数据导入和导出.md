## 导入数据

### mysql命令导入

```sql
mysql -u 用户名 -p 密码 < /home/abc/源.sql
```

### source命令导入

```sql
mysql> source /home/abc/源.sql
```



## 导出数据

### mysqldump命令导出

```bash
mysqldump -u 用户名 -p 密码 数据库名 表名 > 导出文件路径
```

### select ... into outfile命令导出

```sql
SELECT * 
FROM 表名
INTO OUTFILE 导出文件路径
```



