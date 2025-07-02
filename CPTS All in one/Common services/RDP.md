```sh
shakir0x00@htb[/htb]$ nmap -sV -sC 10.129.201.248 -p3389 --script rdp*
```

```sh
nmap -sV -sC 10.129.201.248 -p3389 --packet-trace --disable-arp-ping -n
```


## RDP Security Check

```sh
git clone https://github.com/CiscoCXSecurity/rdp-sec-check.git && cd rdp-sec-check
./rdp-sec-check.pl 10.129.201.248
```

## Initiate an RDP Session

```sh
xfreerdp /u:cry0l1t3 /p:"P455w0rd!" /v:10.129.201.248
```
