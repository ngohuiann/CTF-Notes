# Bash Scripting
## checking users and groups
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

## ping
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
