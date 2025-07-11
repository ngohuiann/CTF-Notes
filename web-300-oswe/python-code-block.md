# Python Code Block

## HTTP Proxy + Simple Request

{% code overflow="wrap" lineNumbers="true" %}
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
