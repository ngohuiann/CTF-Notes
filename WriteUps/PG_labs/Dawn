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

