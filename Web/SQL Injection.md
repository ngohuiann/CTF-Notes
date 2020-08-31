# SQL Injection
## The basics

How an unsecure sql statement looks like:

```
$SQL_Statement = "SELECT * FROM [tablename] WHERE [username] = " + $_POST["username"] + " AND " + $_POST['password']";
```
