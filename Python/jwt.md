```python
import jwt

secret = "JG6gwikhz1Iwt7ZdcdAM"
payload = dict(name="max", age=26)
encoded = jwt.encode(payload=payload, key=secret, algorithm="HS256")
# b'eyJ0eXAiOiJKV1QiLCJ...
decoded = jwt.decode(jwt=encoded, key=secret, algorithms=["HS256"])
# {'name': 'max', 'age': 26} 
```