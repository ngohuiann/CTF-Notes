# Remote Debugging

## Python

```bash
# Python Visual Studio Debugger engine
pipx install ptvsd

# In app.py file add (Default port 5678):
import ptvsd

ptvsd.enable_attach(redirect_output=True)
ptvsd.wait_for_attach()


```
