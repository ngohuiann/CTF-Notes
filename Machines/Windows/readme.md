### SAM (Security Account Manager)
A file that store Windows user credential. It can only be accessed when the Windows OS is not booted up. (Eg. from another OS) <br />
Passwords are LM & NTLM hashed <br />
```
Default file location: 
C:\Windows\System32\config\SAM
C:\Windows\System32\config\SYSTEM

Dumping hashes:
samdump2 SYSTEM SAM

Format of Hashes:
[USERNAME]:[RID]:[LMHash]:[NTLMHash]:::   # RID = Relative Identifier
```

Ref: https://techgenix.com/how-cracked-windows-password-part2/

### Active Directory & LDAP
Enum
```
nmap -n -sV --script "ldap* and not brute" [IP_ADDR]
```
Ref: https://book.hacktricks.xyz/pentesting/pentesting-ldap

### PowerShell history check
```
C:\Users\[username]\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt
```

### Kerberos
```
getnpusers.py     # get user that does not require kerberos preauth
```
Ref: https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetNPUsers.py

### NFS (Network File System)
```
showmount -e [IP_ADDR]
mount -t nfs [IP_ADDR]:/[Remote_Folder] [Local_Folder]/
```
