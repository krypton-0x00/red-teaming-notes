#tip: We need to use more than two tools for enumeration. Because it can happen that due to the programming of the tools, we get different information that we have to check manually. Therefore, we should never rely only on automated tools where we do not know precisely how they were written.

```rb
nmap --script smb-os-discovery.nse -p445 10.10.10.40
```



## Shares 
```rb 
smbclient -N -L \\\\10.129.42.253
```

 Attempt to connect as the guest user.
 
```rb
 smbclient \\\\10.129.42.253\\users
```

Using credentials for the user bob (`bob:Welcome1`).

```rb
smbclient -U bob \\\\10.129.42.253\\users
```

## RPCclient 
This is a tool to perform MS-RPC functions.

```rb
shakir0x00@htb[/htb]$ rpcclient -U "" 10.129.14.128

Enter WORKGROUP\'s password:
rpcclient $> 
```

```shell-session
rpcclient $> srvinfo
rpcclient $> enumdomains
rpcclient $> querydominfo
rpcclient $> netshareenumall		
rpcclient $> netsharegetinfo notes
rpcclient $> enumdomusers
rpcclient $> queryuser 0x3e9
rpcclient $> querygroup 0x201


```

## Brute Forcing User RIDs

```sh
shakir0x00@htb[/htb]$ for i in $(seq 500 1100);do rpcclient -N -U "" 10.129.14.128 -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done

        User Name   :   sambauser
        user_rid :      0x1f5
        group_rid:      0x201
		
        User Name   :   mrb3n
        user_rid :      0x3e8
        group_rid:      0x201
		
        User Name   :   cry0l1t3
        user_rid :      0x3e9
        group_rid:      0x201
```

#tip: An alternative to this would be a Python script from [Impacket](https://github.com/SecureAuthCorp/impacket) called [samrdump.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/samrdump.py).


## What Domains are there.
```sh
shakir0x00@htb[/htb]$ samrdump.py 10.129.14.128

```

## SMBmap

```sh
shakir0x00@htb[/htb]$ smbmap -H 10.129.14.128
```

## CrackMapExec

```sh
shakir0x00@htb[/htb]$ crackmapexec smb 10.129.14.128 --shares -u '' -p ''
```

## Enum4Linux-ng 

```sh
shakir0x00@htb[/htb]$ ./enum4linux-ng.py 10.129.14.128 -A
```

