```sh
sudo nmap 10.129.14.128 -sV -sC -p3306 --script mysql*
```

## Interaction with the MySQL Server
```
 mysql -u root -h 10.129.14.132
```

Some times ssl errors occur.
FIX: 
	`mariadb -u robin -probin -h 10.129.26.127 --ssl --ssl-verify-server-cert=FALSE

