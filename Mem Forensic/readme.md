# Memory Forensic

## Volatility2
### Windows
```
vol.py -f [IMAGE] imageinfo
vol.py -f [IMAGE] --profile=[PROFILE] pslist
vol.py -f [IMAGE] --profile=[PROFILE] psxview
vol.py -f [IMAGE] --profile=[PROFILE] connscan
vol.py -f [IMAGE] --profile=[PROFILE] sockets
vol.py -f [IMAGE] --profile=[PROFILE] netscan
vol.py -f [IMAGE] --profile=[PROFILE] cmdscan   # when cmd.exe in the process dump
vol.py -f [IMAGE] --profile=[PROFILE] consoles
vol.py -f [IMAGE] --profile=[PROFILE] procdump -p [PID] --dump-dir [DEST]
vol.py -f [IMAGE] --profile=[PROFILE] memdump -p [PID] --dump-dir [DEST]
vol.py -f [IMAGE] --profile=[PROFILE] dlllist   # -p to specify process
vol.py -f [IMAGE] --profile=[PROFILE] filescan | grep [FILENAME]
vol.py -f [IMAGE] --profile=[PROFILE] dumpfiles -Q [0x000000001e8feb70] -D [DEST]
vol.py -f [IMAGE] --profile=[PROFILE] shellbags   # get list of last accessed directories
vol.py -f [IMAGE] --profile=[PROFILE] malfind   # malware
vol.py -f [IMAGE] --profile=[PROFILE] mftparser   # deleted files
vol.py -f [IMAGE] --profile=[PROFILE] envars    # environmental variable
```

## Git
```
git show [LOGS_NUMBERS]
```

## Chrome
https://www.foxtonforensics.com/browser-history-examiner/chrome-history-location \n
https://www.foxtonforensics.com/browser-history-examiner/firefox-history-location
