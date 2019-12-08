## request
```python
 headers = {
    "Authorization": "Bearer %s" % getToken(),
    "Content-Type": "text/xml"
  }
  response = requests.post(url, data.encode("utf-8"), headers=headers)
  if response.status_code == 200:
    result = response.json()
    print("push success")
  else:
    print("push fail")
```
