
![SQL Server Management Studio showing Object Explorer with 'Employees' database expanded, displaying tables, views, and other database objects.](https://academy.hackthebox.com/storage/modules/112/ssms.png)

TOOLS: 
[mssql-cli](https://docs.microsoft.com/en-us/sql/tools/mssql-cli?view=sql-server-ver15)
[SQL Server PowerShell](https://docs.microsoft.com/en-us/sql/powershell/sql-server-powershell?view=sql-server-ver15)
[HeidiSQL](https://www.heidisql.com)
[SQLPro](https://www.macsqlclient.com)
[Impacket's mssqlclient.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/mssqlclient.py)

## (IMPT)
Authentication being set to `Windows Authentication` means that the underlying Windows OS will process the login request and use either the local SAM database or the domain controller (hosting Active Directory) before allowing connectivity to the database management system. 

This means if we have local use password we can access the DB.


```sh
sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248
```


## MSF Module

```sh
auxiliary(scanner/mssql/mssql_ping)
```

## Mssqlclient.py

```
python3 mssqlclient.py Administrator@10.129.201.248 -windows-auth



SQL> select name from sys.databases
```


