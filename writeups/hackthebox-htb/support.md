# Support

## Enumeration

### Nmap

```bash
Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-15 10:28 +08
Nmap scan report for 10.10.11.174
Host is up (0.027s latency).
Not shown: 989 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2022-09-15 02:29:00Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: support.htb0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: support.htb0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2016 (85%)
OS CPE: cpe:/o:microsoft:windows_server_2016
Aggressive OS guesses: Microsoft Windows Server 2016 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -1s
| smb2-time: 
|   date: 2022-09-15T02:29:07
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required

TRACEROUTE (using port 139/tcp)
HOP RTT      ADDRESS
1   33.00 ms 10.10.14.1
2   33.00 ms 10.10.11.174
```

### LDAP (389 & 636)

```shell
ldapsearch -H ldap://10.10.11.174 -x -s base namingcontexts                                                  1 ⨯
# extended LDIF
#
# LDAPv3
# base <> (default) with scope baseObject
# filter: (objectclass=*)
# requesting: namingcontexts 
#

#
dn:
namingcontexts: DC=support,DC=htb
namingcontexts: CN=Configuration,DC=support,DC=htb
namingcontexts: CN=Schema,CN=Configuration,DC=support,DC=htb
namingcontexts: DC=DomainDnsZones,DC=support,DC=htb
namingcontexts: DC=ForestDnsZones,DC=support,DC=htb

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1

```

ldapsearch result shown that the domain is support.htb. However, other ldap queries failed. Error message shows user credential is needed to perform the query.

```shell
ldapsearch -H ldap://10.10.11.174 -x -b "DC=support,DC=htb" '(objectClass=Person)'
# extended LDIF
#
# LDAPv3
# base <DC=support,DC=htb> with scope subtree
# filter: (objectClass=Person)
# requesting: ALL
#

# search result
search: 2
result: 1 Operations error
text: 000004DC: LdapErr: DSID-0C090A5A, comment: In order to perform this opera
 tion a successful bind must be completed on the connection., data 0, v4f7c

# numResponses: 1
```

### SMB (445)

Enumerate shares permissions as non-existent user.

{% code overflow="wrap" %}
```shell
smbmap -H 10.10.11.174 -u 'root'
[+] Guest session   	IP: 10.10.11.174:445	Name: 10.10.11.174                                      
 Disk                            Permissions	Comment
 ----                            -----------	-------
 ADMIN$                          NO ACCESS	Remote Admin
 C$                              NO ACCESS	Default share
 IPC$                            READ ONLY	Remote IPC
 NETLOGON                        NO ACCESS	Logon server share 
 support-tools                   READ ONLY	support staff tools
 SYSVOL                          NO ACCESS	Logon server share
```
{% endcode %}

Enumerate readable shares "support-tools".

```shell
smbmap -H 10.10.11.174 -u 'root' -R support-tools
[+] Guest session   	IP: 10.10.11.174:445	Name: 10.10.11.174            
 Disk                                               Permissions	 Comment
 ----                                               -----------	 -------
 support-tools                                      READ ONLY	
 .\support-tools\*
 dr--r--r--            0 Thu Jul 21 01:01:06 2022   .
 dr--r--r--            0 Sat May 28 19:18:25 2022   ..
 fr--r--r--      2880728 Sat May 28 19:19:19 2022   7-ZipPortable_21.07.paf.exe
 fr--r--r--      5439245 Sat May 28 19:19:55 2022   npp.8.4.1.portable.x64.zip
 fr--r--r--      1273576 Sat May 28 19:20:06 2022   putty.exe
 fr--r--r--     48102161 Sat May 28 19:19:31 2022   SysinternalsSuite.zip
 fr--r--r--       277499 Thu Jul 21 01:01:07 2022   UserInfo.exe.zip
 fr--r--r--        79171 Sat May 28 19:20:17 2022   windirstat1_1_2_setup.exe
 fr--r--r--     44398000 Sat May 28 19:19:43 2022   WiresharkPortable64_3.6.5.paf.exe
```

### UserInfo.exe

UserInfo.exe.zip looks suspicious. Download and unzip to retrieve the zip content.

```shell
ls -la
total 672
drwxr-xr-x 2 root root   4096 Sep 15 10:16 .
drwxr-xr-x 3 root root   4096 Sep 14 15:25 ..
-rw-r--r-- 1 root root  99840 Mar  2  2022 CommandLineParser.dll
-rw-r--r-- 1 root root  22144 Oct 23  2021 Microsoft.Bcl.AsyncInterfaces.dll
-rw-r--r-- 1 root root  47216 Oct 23  2021 Microsoft.Extensions.DependencyInjection.Abstractions.dll
-rw-r--r-- 1 root root  84608 Oct 23  2021 Microsoft.Extensions.DependencyInjection.dll
-rw-r--r-- 1 root root  64112 Oct 23  2021 Microsoft.Extensions.Logging.Abstractions.dll
-rw-r--r-- 1 root root  20856 Feb 19  2020 System.Buffers.dll
-rw-r--r-- 1 root root 141184 Feb 19  2020 System.Memory.dll
-rw-r--r-- 1 root root 115856 May 15  2018 System.Numerics.Vectors.dll
-rw-r--r-- 1 root root  18024 Oct 23  2021 System.Runtime.CompilerServices.Unsafe.dll
-rw-r--r-- 1 root root  25984 Feb 19  2020 System.Threading.Tasks.Extensions.dll
-rwxr-xr-x 1 root root  12288 May 28 01:51 UserInfo.exe
-rw-r--r-- 1 root root    563 May 28 00:59 UserInfo.exe.config
```

UserInfo.exe.config file content shows that the program is written with .NETFramework version 4.8

```xml
cat UserInfo.exe.config 
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.8" />
    </startup>
  <runtime>
    <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
      <dependentAssembly>
        <assemblyIdentity name="System.Runtime.CompilerServices.Unsafe" publicKeyToken="b03f5f7f11d50a3a" culture="neutral" />
        <bindingRedirect oldVersion="0.0.0.0-6.0.0.0" newVersion="6.0.0.0" />
      </dependentAssembly>
    </assemblyBinding>
  </runtime>
</configuration>
```

Use [dnSpy](https://github.com/dnSpy/dnSpy) to decompile UserInfo.exe. In UserInfo.Services -> LdapQuery, it is shown that the username is "ldap" and domain is "support".

<pre class="language-csharp"><code class="lang-csharp">public LdapQuery()
{
    string password = Protected.getPassword();
<strong>    this.entry = new DirectoryEntry("LDAP://support.htb", "support\\ldap", password);
</strong>    this.entry.AuthenticationType = AuthenticationTypes.Secure;
    this.ds = new DirectorySearcher(this.entry);
}</code></pre>

In the UserInfo.Services, Protected, a password encryption function is shown.

```csharp
using System;
using System.Text;

namespace UserInfo.Services
{
	// Token: 0x02000006 RID: 6
	internal class Protected
	{
		// Token: 0x0600000F RID: 15 RVA: 0x00002118 File Offset: 0x00000318
		public static string getPassword()
		{
			byte[] array = Convert.FromBase64String(Protected.enc_password);
			byte[] array2 = array;
			for (int i = 0; i < array.Length; i++)
			{
				array2[i] = (array[i] ^ Protected.key[i % Protected.key.Length] ^ 223);
			}
			return Encoding.Default.GetString(array2);
		}

		// Token: 0x04000005 RID: 5
		private static string enc_password = "0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E";

		// Token: 0x04000006 RID: 6
		private static byte[] key = Encoding.ASCII.GetBytes("armando");
	}
}
```

An [online C# compiler](https://dotnetfiddle.net/) can be used to recompile the code and print the decrypted password.

```csharp
using System;
using System.Text;

public class Example {
    public static void Main()
    {
	string enc_password = "0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E";
	byte[] key = Encoding.ASCII.GetBytes("armando");
	byte[] array = Convert.FromBase64String(enc_password);
	byte[] array2 = array;
		for (int i = 0; i < array.Length; i++)
		{
			array2[i] = Convert.ToByte(array[i] ^ key[i % key.Length] ^ 223);
		}
	//return Encoding.Default.GetString(array2);
	Console.WriteLine(Encoding.Default.GetString(array2));
	}
}
```

## User

### LDAP query



