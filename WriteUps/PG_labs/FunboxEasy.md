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
