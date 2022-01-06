# Config Files

Config files often contain passwords in plaintext or other reversible formats.

List the contents of the user's home directory:

`ls /home/user`

Note the presence of a **myvpn.ovpn** config file. View the contents of the file:

`cat /home/user/myvpn.ovpn`

The file should contain a reference to another location where the root user's credentials can be found. Switch to the root user, using the credentials:

`su root`
