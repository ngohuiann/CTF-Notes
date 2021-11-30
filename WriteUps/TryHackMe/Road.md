# NMAP
```
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 61 OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 e6:dc:88:69:de:a1:73:8e:84:5b:a1:3e:27:9f:07:24 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDXhjztNjrxAn+QfSDb6ugzjCwso/WiGgq/BGXMrbqex9u5Nu1CKWtv7xiQpO84MsC2li6UkIAhWSMO0F//9odK1aRpPbH97e1ogBENN6YBP0s2z27aMwKh5UMyrzo5R42an3r6K+1x8lfrmW8VOOrvR4pZg9Mo+XNR/YU88P3XWq22DNPJqwtB3q4Sw6M/nxxUjd01kcbjwd1d9G+nuDNraYkA2T/OTHfp/xbhet9K6ccFHoi+A8r6aL0GV/qqW2pm4NdfgwKxM73VQzyolkG/+DFkZc+RCH73dYLEfVjMjTbZTA+19Zd2hlPJVtay+vOZr1qJ9ZUDawU7rEJgJ4hHDqlVjxX9Yv9SfFsw+Y0iwBfb9IMmevI3osNG6+2bChAtI2nUJv0g87I31fCbU5+NF8VkaGLz/sZrj5xFvyrjOpRnJW3djQKhk/Avfs2wkZ+GiyxBOZLetSDFvTAARmqaRqW9sjHl7w4w1+pkJ+dkeRsvSQlqw+AFX0MqFxzDF7M=
|   256 6b:ea:18:5d:8d:c7:9e:9a:01:2c:dd:50:c5:f8:c8:05 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNBLTibnpRB37eKji7C50xC9ujq7UyiFQSHondvOZOF7fZHPDn3L+wgNXEQ0wei6gzQfiZJmjQ5vQ88vEmCZzBI=
|   256 ef:06:d7:e4:b1:65:15:6e:94:62:cc:dd:f0:8a:1a:24 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPv3g1IqvC7ol2xMww1gHLeYkyUIe8iKtEBXznpO25Ja
80/tcp open  http    syn-ack ttl 61 Apache httpd 2.4.41 ((Ubuntu))
|_http-favicon: Unknown favicon MD5: FB0AA7D49532DA9D0006BA5595806138
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
|_http-title: Sky Couriers
```

# Gobuster
http://[IP_ADDR]/
```
gobuster dir -u http://[IP]/ -w /opt/wordlists/dirb/big.txt -x php,html -t 50
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.40.242/
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /opt/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              php,html
[+] Timeout:                 10s
===============================================================
2021/11/30 09:38:32 Starting gobuster in directory enumeration mode
===============================================================
/assets               (Status: 301) [Size: 313] [--> http://10.10.40.242/assets/]
/career.html          (Status: 200) [Size: 9289]                                 
/index.html           (Status: 200) [Size: 19607]                                
/phpMyAdmin           (Status: 301) [Size: 317] [--> http://10.10.40.242/phpMyAdmin/]
/server-status        (Status: 403) [Size: 277]                                      
/v2                   (Status: 301) [Size: 309] [--> http://10.10.40.242/v2/]        
                                                                                     
===============================================================
2021/11/30 09:47:16 Finished
===============================================================
```
<br />

http://[IP_ADDR]/v2/

```
gobuster dir -u http://[IP]/v2 -w /opt/wordlists/dirb/big.txt -x php,html -t 50
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.40.242/v2
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /opt/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              php,html
[+] Timeout:                 10s
===============================================================
2021/11/30 09:34:44 Starting gobuster in directory enumeration mode
===============================================================
/admin                (Status: 301) [Size: 315] [--> http://10.10.40.242/v2/admin/]
/index.php            (Status: 302) [Size: 20178] [--> /v2/admin/login.html]       
/lostpassword.php     (Status: 200) [Size: 22]                                     
/profile.php          (Status: 302) [Size: 26751] [--> /v2/admin/login.html]       
                                                                                   
===============================================================
2021/11/30 09:43:36 Finished
===============================================================
```

<br />
http://[IP_ADDR]/v2/admin/

```
gobuster dir -u http://[IP]/v2/admin -w /opt/wordlists/dirb/big.txt -x php,html -t 50
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.40.242/v2/admin
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /opt/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              php,html
[+] Timeout:                 10s
===============================================================
2021/11/30 09:44:06 Starting gobuster in directory enumeration mode
===============================================================
/index.php            (Status: 200) [Size: 32] 
/login.html           (Status: 200) [Size: 2619]
/logout.php           (Status: 302) [Size: 0] [--> login.html]
/reg.php              (Status: 200) [Size: 28]                
/register.html        (Status: 200) [Size: 3798]              
                                                              
===============================================================
2021/11/30 09:52:44 Finished
===============================================================
```

Admin email: admin@sky.thm found in edit profile page. <br />
Reset admin email to enabled profile image upload and get reverse shell.

# Priv Esc
LD_Preload environment variable
https://www.hackingarticles.in/linux-privilege-escalation-using-ld_preload/
