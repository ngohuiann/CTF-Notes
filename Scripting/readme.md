# Scripting

## Bash Scripting

### checking users and groups

```bash
#!/bin/bash

read -p 'Username: ' username
read -p 'Group: ' groupname

userexist=$(cat /etc/passwd | grep $username | awk -F ':'  '{print $1}')    # example result if exists: root ; if not exists ""
groupexist=$(cat /etc/group | awk -F ':'  '{print $1}' | grep $groupname)   # example result if exists: root ; if not exists ""

if [[ $username = $userexist ]] && [[ $groupname = $groupexist ]] 
then
	result=$(cat /etc/passwd | grep $groupexist)
	if [ $result = "" ]
	then
		echo "Invalid"
	else
		echo "Valid"
	fi
elif [[ $username = $userexist ]] && [[ $groupname != $groupexist ]]
then
	echo 'Exists'
elif [[ $groupname = $groupexist ]] && [[ $username != $userexist ]]
then
	echo 'Exists'
else 
	echo 'Not found'
fi
```

Logic flow:

1. it first prompt user for username and group and save both input into variable "username" and "groupname".
2. It then check if the username and groupname provided by user matches the record in /etc/passwd and /etc/group.
3. IF condition is pretty much straight forward.

### ping

```bash
#!/bin/bash

# Example ./ping 192.168.6 1 254
# Output list of pingable IP

counter=$2

while [ $counter -le $3 ]
do
	result=$(ping -w 2 -c 1 $1.$counter | grep "bytes from")    # -w 2 set the reply wait time to 2 seconds. May increase if network is slow.
	if [[ $result != "" ]]
	then
		echo $result | awk '{print $4}' | cut -f1 -d":"
		((counter++))
	else
		((counter++))
	fi
done
```

## Python3 Scripting

### ping sweep

```
#!/usr/bin/env python3

# ./test 192.168.6 1 10

import subprocess
import sys

ip=sys.argv[1]
counter=sys.argv[2]
end=sys.argv[3]

while int(counter) <= int(end):
    pingcmd = 'ping -w 2 -c 1 ' + str(ip) + '.' + str(counter) 
    grepcmd = 'grep "bytes from"'
    p = subprocess.Popen(pingcmd, shell=True, stdout=subprocess.PIPE)
    r = subprocess.Popen(grepcmd, shell=True, stdin=p.stdout, stdout=subprocess.PIPE)
    p.stdout.close()
    rresult = r.stdout.read()
    if rresult != b'':
        print(str(ip) + '.' + str(counter))
        counter= int(counter)+1
    else:
        counter= int(counter)+1
```

### grabing .js filename inside access log

```
#!/usr/bin/env python3

# ./test accesslog.txt

import re
import sys

file=sys.argv[1]

pattern='(.*?\.js.*?)'
pattern1=".*/"
result = []

f = open(file, "r")

for lines in f:
    if re.search(pattern, lines):
        line = lines.split(' ')[6]
        final = re.sub(pattern1, '', line )
        result.append(final)

uniq=[]
for x in result: 
    if x not in uniq:
        uniq.append(x)

uniq.sort()

print(uniq)
```
