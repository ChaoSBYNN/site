---
title: >-
  MySQL: Expression #1 of SELECT list is not in GROUP BY clause and contains
  nonaggregated column 
date: 2024-07-05 08:55:54
tags: 
    - "MySQL"
categories:
    - "Program"
    - "MySQL"
cover: "https://raw.githubusercontent.com/ChaoSBYNN/image-hosting/main/program/mysql.jpg"
excerpt: "MySQL GROUP BY 异常"
---

```ini
Error querying database.  Cause: java.sql.SQLSyntaxErrorException: Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'db.table.id' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
```

这个错误通常是因为 MySQL 数据库的 `sql_mode` 设置了 `only_full_group_by`，导致在执行 SQL 查询时，必须将 GROUP BY 子句中的每个非聚合列都包含在 SELECT 列表中。如果查询中的 SELECT 列表包含非聚合列，但没有在 GROUP BY 子句中指定，就会导致这个错误。

## 解决方法: 

### 方法一: 调整查询语句
在 SQL 查询中，确保将 SELECT 列表中的每个列都包含在 GROUP BY 子句中。如果查询是这样的: 

```sql
SELECT id, name, MAX(salary) AS max_salary
FROM employees
GROUP BY name;
```

要解决问题，应该将 id 列也包含在 GROUP BY 子句中: 

```sql
SELECT id, name, MAX(salary) AS max_salary
FROM employees
GROUP BY id, name;
```

### 方法二: 修改 MySQL 的 sql_mode

如果不方便修改查询语句或者需要保持现有查询逻辑，可以修改 MySQL 的 sql_mode，移除 only_full_group_by。这样做需要注意，因为 only_full_group_by 是 MySQL 的一种严格模式，移除它可能会导致在某些情况下出现不符合预期的查询结果。

可以通过以下步骤来修改 MySQL 的 sql_mode: 

打开 MySQL 配置文件，通常位于 `/etc/mysql/my.cnf` 或 `/etc/my.cnf`。

找到 sql_mode 配置项，类似于: 

```ini
sql_mode = "STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
```

移除其中的 only_full_group_by，或者将整个 sql_mode 设置为适合的值。

重启 MySQL 服务使修改生效: 

```bash
sudo systemctl restart mysql
```

请注意，在生产环境中修改 sql_mode 之前，请务必进行充分的测试，并确保了解可能带来的影响。

### 方法三: 使用 MySQL 的 ANY_VALUE() 函数

在 MySQL 5.7 版本及以上，可以使用 ANY_VALUE() 函数来解决这个问题。这个函数可以避免严格模式下 GROUP BY 子句中非聚合列的问题。例如: 

```sql
SELECT ANY_VALUE(id), name, MAX(salary) AS max_salary
FROM employees
GROUP BY name;
```

## 方法四: 使用 SQL 设置 sql_mode

命令行 设置 `sql_mode=only_full_group_by`

在登录到 MySQL 后，可以执行以下命令查看当前的 sql_mode 设置: 

```sql
SELECT @@sql_mode;
```

这将显示当前的 sql_mode 配置。

设置新的 sql_mode

要设置 sql_mode，可以使用 SET GLOBAL 或 SET SESSION 命令。一般来说，全局设置需要超级用户权限，而会话级别设置只对当前连接有效，不需要特殊权限。

设置会话级别的 sql_mode: 

```sql
SET SESSION sql_mode = 'ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,...';  -- 添加或替换需要的 sql_mode 值
```

设置全局级别的 sql_mode（需要超级用户权限）: 

```sql
SET GLBAL sql_mode = 'ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,...';  -- 添加或替换需要的 sql_mode 值
```

> 注意: 全局级别的设置会影响所有新连接到 MySQL 的会话，而会话级别的设置仅对当前会话有效。

验证设置是否生效

设置完成后，可以再次执行 `SELECT @@sql_mode;` 命令来验证新的 sql_mode 是否已生效。

永久修改配置文件

如果希望永久修改 sql_mode，可以编辑 MySQL 的配置文件（如 /etc/mysql/my.cnf 或 /etc/my.cnf），找到 sql_mode 配置项，并修改为需要的值。然后重启 MySQL 服务使修改生效: 

```bash
sudo systemctl restart mysql
```

这样修改后，MySQL 将会在每次启动时使用新的 sql_mode 设置。

> 注意事项:
> 修改 sql_mode 可能会影响到现有的应用程序和查询行为，请确保在生产环境中进行充分测试。
> 建议仅修改和删除有必要的 sql_mode 设置，以确保数据库的安全性和一致性。
> 在修改全局级别的 sql_mode 时需要特别注意，避免对其他应用产生不必要的影响。
> 通过以上步骤，可以在命令行中设置 MySQL 的 sql_mode，解决与 only_full_group_by 相关的 SQL 查询问题。
这样可以避免 id 列不在 GROUP BY 子句中的问题。

## 总结

根据具体情况，选择适合的方法来解决 `Expression #1 of SELECT list is not in GROUP BY clause` 错误。一般建议优先考虑调整查询语句或使用 `ANY_VALUE()` 函数来符合严格的 `sql_mode` 要求，保持数据库的安全性和一致性。
