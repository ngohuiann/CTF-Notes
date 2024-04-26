# Web

1. [Access Control](access\_control.md)
2. [LFI](lfi-rfi.md)
3. [Reverse Shell](reverse\_shell.md)
4. [SQL Injection](sql\_injection.md)
5. [Session Attack](session\_attack.md)
6. [SSTI](ssti.md)
7. [XSS Cross Site Scripting](xss.md)
8. [XXE](xxe.md)

***

## Basic Terminology

HTTP/1.0 - [RFC 1945](https://datatracker.ietf.org/doc/html/rfc1945)

HTTP/1.1 - [RFC 9112](https://datatracker.ietf.org/doc/html/rfc9112)



### Springboot Actuator Endpoint

If the /actuator/heapdump is available for download, it is possible to view the cleartext password from the heapdump file using [Eclipse Memory Analyzer](https://eclipse.dev/mat/downloads.php).

**Eclipse Memory Analyzer (Tools):** https://eclipse.dev/mat/downloads.php&#x20;

**Object Query Language (OQL):** select \* from java.util.LinkedHashMap$Entry x WHERE (toString(x.key).contains("password"))
