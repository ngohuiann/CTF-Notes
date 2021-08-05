## Important files (Default location)
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
/etc/apache2/sites-available/000-default.conf
/etc/samba/smb.conf
```

### PrivEsc
```
sudo -l   # /etc/sudoers
find / -perm -4000 2>/dev/null    # file with suid

```
