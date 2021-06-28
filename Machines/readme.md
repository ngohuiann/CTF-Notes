# Recon & Enum
## The basic
Data and information gathering.

## [nmap](https://nmap.org/book/man.html)
1. Host scan
```
nmap -p 1-65535 -v -A -Pn [IP_ADDR]
nmap -sC -sV -A -p- -v [IP_ADDR]
```

## Commonly seen port (Default port num)
### FTP (21)
```
ftp [IP_ADDR]
anonymous sign in
```

### SSH (22)
```
ssh [username]@[IP_ADDR]
```

### HTTP (80)

### HTTPS (443)

### SMB (445)
```
smbclient -L [IP_ADDR]    # To list out available directories
smbclient -D \\\\[IP_ADDR]\\[DIRECTORY_NAME]    # To show directory content
```
MS17-010 EternalBlue exploitation for SMBv1 in Windows Vista, 7, 8.1, 10; Server 2008, 2012, 2016

### Microsoft SQL (1433)
```
sqlcmd -S [IP_ADDR] -U [USERNAME] -P [PASSWORD]
```

### MySQL (3306)
```
mysql -u [username] -h [IP_ADDR] -p
```

### RDP (3389)

### SPLUNK (8089)
```
Default Credential 
admin:changeme
```
