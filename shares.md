# Shares

```

#smb 435 
sudo smbclient -L \\\\192.168.1.4\\anonymous -U anonymous 
sudo smbclient \\\\192.168.1.4\\anonymous

#ftp 21
ftp ip-address

#nfc tcp/udp 111,2049
showmount -e ip-address
mount ip:/file/path /local/file/path
umount /local/file/path

#mssql 3306
mysql -h 10.10.125.220 -u root -p
#SHOW databases; : Prints out the databases we can access.
#USE {database_name}; : Set to use the database named {database_name}.
#SHOW tables; : Prints out the available tables inside the current database.
#SELECT * FROM {table_name}; : Prints out all the data from the table {table_name}.

```
