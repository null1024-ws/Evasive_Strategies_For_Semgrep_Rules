### Code Snippet
``` python
from Crypto.Ciphers import AES

def bad1():
    cipher = AES.new("", AES.MODE_CFB, iv)
    msg = iv + cipher.encrypt(b'Attack at dawn')
```
### Transformation 1*
``` python
from Crypto.Ciphers import AES

def bad1():
    cipher = AES.new("freecode"[0,0], AES.MODE_CFB, iv)
    msg = iv + cipher.encrypt(b'Attack at dawn')
```
