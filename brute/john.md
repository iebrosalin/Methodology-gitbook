# John

```
#john
john --wordlis=./rockyou.txt shadow  

zip2john backup.zip > hash
john -wordlist=/usr/share/wordlists/rockyou.txt hashes
john --show hash
```
