Communicate unencrypted over the network (like Telnet) Replaced today by SSH.

PORTS:  512,513,514

```sh
sudo nmap -sV -p 512,513,514 10.0.17.2
```
## Logging in Using Rlogin

```rb
rlogin 10.0.17.2 -l htb-student

[htb-student@localhost ~]$
```

#### Listing Authenticated Users Using Rwho

```rb
shakir0x00@htb[/htb]$ rwho

root     web01:pts/0 Dec  2 21:34
htb-student     workstn01:tty1  Dec  2 19:57  2:25 
```

#tip: From this information, we can see that the `htb-student` user is currently authenticated to the `workstn01` host, whereas the `root` user is authenticated to the `web01` host. We can use this to our advantage when scoping out potential usernames to use during further attacks on hosts over the network. 

## Listing Authenticated Users Using Rusers

```rb
shakir0x00@htb[/htb]$ rusers -al 10.0.17.5

htb-student     10.0.17.5:console          Dec 2 19:57     2:25
```



