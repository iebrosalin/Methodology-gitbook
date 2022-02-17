# Gobuster

{% embed url="https://github.com/OJ/gobuster" %}
h1
{% endembed %}

brute dir

```
gobuster dir -u http://10.10.144.246:3000 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt  2>/dev/null
```

dns brute

```
gobuster dir -d http://10.10.144.246:3000 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt  2>/dev/null
```
