# Memory Forensic

## Volatility2
### Windows
```
vol.py -f [IMAGE].raw imageinfo
vol.py -f [IMAGE].raw --profile=[PROFILE] pslist
vol.py -f [IMAGE].raw --profile=[PROFILE] psxview
vol.py -f [IMAGE].raw --profile=[PROFILE] connscan
vol.py -f [IMAGE].raw --profile=[PROFILE] sockets
vol.py -f [IMAGE].raw --profile=[PROFILE] netscan
vol.py -f [IMAGE].raw --profile=[PROFILE] cmdscan   # when cmd.exe in the process dump
vol.py -f [IMAGE].raw --profile=[PROFILE] consoles
vol.py -f [IMAGE].raw --profile=[PROFILE] procdump -p [PID] --dump-dir [DEST]
vol.py -f [IMAGE].raw --profile=[PROFILE] memdump -p [PID] --dump-dir [DEST]
vol.py -f [IMAGE].raw --profile=[PROFILE] dlllist   # -p to specify process
vol.py -f [IMAGE].raw --profile=[PROFILE] filescan | grep [FILENAME]
```

## Git
```
git show [LOGS_NUMBERS]
```
