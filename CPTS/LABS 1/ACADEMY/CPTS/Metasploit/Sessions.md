```rb
rustscan -a 10.129.50.185 

PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack
80/tcp open  http    syn-ack

```

Running elFInder on port 80.

![[Pasted image 20250629061637.png]]

Got the shell as www-data


### Now for PE
we found old version of sudo being used `1.18.31`
We can easily decide to run the local exploit suggester module, attaching it to the currently active Meterpreter session.

using : 
```shell-session
 post/multi/recon/local_exploit_suggester   
```



USED: `exploit/linux/local/sudo_baron_samedit`

```
cd /root
cat flag.txt => HTB{5e55ion5_4r3_sw33t}
```


