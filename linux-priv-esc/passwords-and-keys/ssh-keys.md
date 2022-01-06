# SSH Keys

Sometimes users make backups of important files but fail to secure them with the correct permissions.

Look for hidden files & directories in the system root:

`ls -la /`

Note that there appears to be a hidden directory called **.ssh**. View the contents of the directory:

`ls -l /.ssh`

Note that there is a world-readable file called **root\_key**. Further inspection of this file should indicate it is a private SSH key. The name of the file suggests it is for the root user.

Copy the key over to your Kali box (it's easier to just view the contents of the **root\_key** file and copy/paste the key) and give it the correct permissions, otherwise your SSH client will refuse to use it:

`chmod 600 root_key`

Use the key to login to the Debian VM as the root account:

`ssh -i root_key root@MACHINE_IP`
