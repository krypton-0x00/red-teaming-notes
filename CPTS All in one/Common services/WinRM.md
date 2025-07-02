WinRM is a built-in **remote management protocol** in Windows that allows you to **control and manage remote computers** using the command line.

```sh
nmap -sV -sC 10.129.201.248 -p5985,5986 --disable-arp-ping -n
```


## Evil WinRm (Interaction win WinRM)

```sh
evil-winrm -i 10.129.201.248 -u Cry0l1t3 -p P455w0rD!
```

