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

### Routing OpenAPI/Swagger JSON file through Burp Proxy

{% code overflow="wrap" %}
```python
import requests
import json, base64, time

requests.packages.urllib3.disable_warnings(requests.packages.urllib3.exceptions.InsecureRequestWarning)
verify_ssl = False

# Load the OpenAPI JSON file
with open("swagger.json", "r") as file:
    openapi_spec = json.load(file)

base_url = "https://example.com"    # CHANGEME
proxies = {"http": "http://127.0.0.1:8080", "https": "http://127.0.0.1:8080"}

verify_ssl = False
token = "example"    # CHANGEME (Authorization: Bearer example)

def resolve_schema(schema_ref):
    schema_name = schema_ref.split("/")[-1]
    
    schema_definition = openapi_spec.get("components", {}).get("schemas", {}).get(schema_name, {})
    if schema_definition.get("properties"):
        resolved_schema = {}
        for prop_name, prop_data in schema_definition["properties"].items():
            prop_type = prop_data.get("type")
            prop_format = prop_data.get("format")
            prop_array_item = prop_data.get("items")
            prop_ref = prop_data.get("$ref")
            
            if prop_ref:
                prop_schema_name = prop_ref.split("/")[-1]
                prop_schema_definition = openapi_spec.get("components", {}).get("schemas", {}).get(prop_schema_name, {})
                if prop_schema_definition.get("properties"):
                    prop_resolved_schema = {}
                    for i_prop_name, i_prop_data in prop_schema_definition["properties"].items():
                        i_prop_type = i_prop_data.get("type")
                        i_prop_format = i_prop_data.get("format")
                        if i_prop_type == "string":
                            if i_prop_format and i_prop_format == "date-time":
                                prop_resolved_schema[i_prop_name] = "2024-04-24T00:58:48.893Z"
                            elif i_prop_format and i_prop_format == "byte":
                                encoded_data = base64.b64encode(b'example_string')
                                prop_resolved_schema[i_prop_name] = encoded_data
                            else:
                                prop_resolved_schema[i_prop_name] = "example_string"
                        elif i_prop_type == "integer":
                            prop_resolved_schema[i_prop_name] = 12
                        elif i_prop_type == "number":
                            prop_resolved_schema[i_prop_name] = 123.45
                        elif i_prop_type == "boolean":
                            prop_resolved_schema[i_prop_name] = True
                        elif i_prop_type == "array":
                            prop_resolved_schema[i_prop_name] = []
                        elif i_prop_type == "object":
                            prop_resolved_schema[i_prop_name] = {}
                        else:
                            prop_resolved_schema[i_prop_name] = None
                else:
                    prop_resolved_schema = 0
                resolved_schema[prop_name] = prop_resolved_schema
            elif prop_type == "string":
                if prop_format and prop_format == "date-time":
                    resolved_schema[prop_name] = "2024-04-24T00:58:48.893Z"
                elif prop_format and prop_format == "byte":
                    encoded_data = base64.b64encode(b'example_string')
                    resolved_schema[prop_name] = encoded_data
                else:
                    resolved_schema[prop_name] = "example_string"
            elif prop_type == "integer":
                resolved_schema[prop_name] = 123
            elif prop_type == "number":
                resolved_schema[prop_name] = 123.45
            elif prop_type == "boolean":
                resolved_schema[prop_name] = True
            elif prop_type == "array":
                items_schema = prop_data.get("items", {})
                if items_schema.get("type"):
                    resolved_schema[prop_name] = [items_schema["type"]]
                elif items_schema.get("$ref") and schema_name != items_schema.get("$ref").split("/")[-1]:
                    item_type = resolve_schema(items_schema["$ref"])
                    resolved_schema[prop_name] = [item_type]
                else:
                    resolved_schema[prop_name] = []
            elif prop_type == "object":
                resolved_schema[prop_name] = {}
            else:
                resolved_schema[prop_name] = None
        return resolved_schema
    else:
        return None

def construct_url_with_params(path, parameters):
    url = base_url + path
    query_params = []
    for param in parameters:
        param_name = param['name']
        param_schema = param['schema']
        if 'example' in param_schema:
            param_value = param_schema['example']
        elif param_schema.get('type') == 'boolean':
            param_value = 'true' if param_schema.get('default', False) else 'false'
        elif param_schema.get('type') == 'integer':
            param_value = str(param_schema.get('default', 0))
        elif param_schema.get('type') == 'string' and param_schema.get('format') == 'date-time':
            param_value = "2024-04-24T00:58:48.893Z"
        else:
            param_value = 'example_string'
        query_params.append(f"{param_name}={param_value}")
    if query_params:
        url += "?" + "&".join(query_params)
    return url
    
for path, path_data in openapi_spec["paths"].items():
    for method, method_data in path_data.items():
        url = construct_url_with_params(path, method_data.get("parameters", []))
        headers = {"Authorization": f"Bearer {token}"}  # Authorization header

        # Prepare the request based on the method
        if method.lower() == "get":
            body_schema_ref = None
            request_body_content = method_data.get("requestBody", {}).get("content", {})
            for content_type, content_data in request_body_content.items():
                if "schema" in content_data:
                    body_schema_ref = content_data["schema"].get("$ref")
                    break
            body_data = None
            if body_schema_ref:
                body_data = resolve_schema(body_schema_ref)
            response = requests.get(url, json=body_data, headers=headers, proxies=proxies, verify=verify_ssl)
        elif method.lower() == "post":
            body_schema_ref = None
            request_body_content = method_data.get("requestBody", {}).get("content", {})
            for content_type, content_data in request_body_content.items():
                if "schema" in content_data:
                    body_schema_ref = content_data["schema"].get("$ref")
                    break
            body_data = None
            if body_schema_ref:
                body_data = resolve_schema(body_schema_ref)
            response = requests.post(url, json=body_data, headers=headers, proxies=proxies, verify=verify_ssl)
        elif method.lower() == "put":
            body_schema_ref = None
            request_body_content = method_data.get("requestBody", {}).get("content", {})
            for content_type, content_data in request_body_content.items():
                if "schema" in content_data:
                    body_schema_ref = content_data["schema"].get("$ref")
                    break
            body_data = None
            if body_schema_ref:
                body_data = resolve_schema(body_schema_ref)
            response = requests.put(url, json=body_data, headers=headers, proxies=proxies, verify=verify_ssl)
        elif method.lower() == "delete":
            body_schema_ref = None
            request_body_content = method_data.get("requestBody", {}).get("content", {})
            for content_type, content_data in request_body_content.items():
                if "schema" in content_data:
                    body_schema_ref = content_data["schema"].get("$ref")
                    break
            body_data = None
            if body_schema_ref:
                body_data = resolve_schema(body_schema_ref)
            response = requests.delete(url, json=body_data, headers=headers, proxies=proxies, verify=verify_ssl)
        else:
            print(f"Unsupported HTTP method: {method}")
            continue

        print(f"Response for {method} {url}: {response.status_code}")
        time.sleep(1)
```
{% endcode %}
