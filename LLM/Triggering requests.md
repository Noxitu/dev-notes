

# Python

```python
import requests
import json

URL = ...
PAYLOAD = ...

def main():
    response = requests.post(URL, json=PAYLOAD)
    print("Status code:", response.status_code)

    if PAYLOAD["stream"]:
        print("Response body:", response.text)
    else:
        print("Response body:", json.dumps(response.json(), indent=4))


if __name__ == "__main__":
    main()

```

# curl 

```
curl <URL> \
    -H "Content-Type: application/json" \
    -d '<PAYLOAD>'
```
