# Abusing SUID/GUID Files



```
find / -perm -u=s -type f  -exec ls -ldb {} \; 2>/dev/null
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null

Let's break down this command:

find - Initiates the "find" command 
/ - Searches the whole file system 
-perm - searches for files with specific permissions 
-u=s - Any of the permission bits mode are set for the file. Symbolic modes are accepted in this form 
-type f - Only search for files 
2>/dev/null - Suppresses errors 
```
