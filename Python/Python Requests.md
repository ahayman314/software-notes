[[Python]] requests is a library used for doing requests. This is complimentary to [[Python Flask]], especially during component testing and integration testing. This is like [[curl & wget]] for [[HTTP - L7]]. 
Install with `pip install requests`
### POST Request Example
```python
import requests

url = 'https://api.example.com'
uri_dict = {"data": "stuff", "more_data": "more_stuff"}
header = {"Authorization": f"Bearer {my_token}"}

response = requests.post(url, json=uri_dict, headers=header, timeout=10)

assert response.status_code == 200
assert "something" in response.json()
stuff = response.json()["something"]

response_content = response.content
```