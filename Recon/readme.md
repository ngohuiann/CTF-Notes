# Reconnaissance
## The basic
Data and information gathering.

## [nmap](https://nmap.org/book/man.html)
1. Basic host scan
```
nmap -v [IP_ADDR]
nmap -v [192.168.1.1-120]
nmap -v [IP_ADDR]/[SUBNET]
```

2. Port scan
```
nmap -p [PORT_NUM] [IP_ADDR]
nmap -p [20-2000] [IP_ADDR]
```

3. Scan all (65535) port 
```
nmap -p- [IP_ADDR]
```

4. Scan without host discovery (-Pn)
```
nmap -p 1-65535 -v -A -Pn [AP_ADDR]
```
