# Reconnaissance
## The basic
Data and information gathering.

## [nmap](https://nmap.org/book/man.html)
1. Basic host scan
<pre>
nmap -v [IP_ADDR]
nmap -v [192.168.1.1-120]
nmap -v [IP_ADDR]/[SUBNET]
</pre>

2. Port scan
<pre>
nmap -p [PORT_NUM] [IP_ADDR]
nmap -p [20-2000] [IP_ADDR]
</pre>

3. Scan all (65535) port 
```
nmap -p- [IP_ADDR]
```
