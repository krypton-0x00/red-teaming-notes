```sh
shakir0x00@htb[/htb]$ sudo nmap 10.129.14.128 -p111,2049 -sV -sC

```

```sh
shakir0x00@htb[/htb]$ sudo nmap --script nfs* 10.129.14.128 -sV -p111,2049
```

## Show Available NFS Shares

```sh 
shakir0x00@htb[/htb]$ showmount -e 10.129.14.128

Export list for 10.129.14.128:
/mnt/nfs 10.129.14.0/24
```

## Mounting NFS Share

```sh
shakir0x00@htb[/htb]$ mkdir target-NFS
shakir0x00@htb[/htb]$ sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock
shakir0x00@htb[/htb]$ cd target-NFS
```

	 List Contents with UIDs & GUIDs
```sh
shakir0x00@htb[/htb]$ ls -n mnt/nfs/

total 16
-rw-r--r-- 1 1000 1000 1872 Sep 25 00:55 cry0l1t3.priv
-rw-r--r-- 1 1000 1000  348 Sep 25 00:55 cry0l1t3.pub
-rw-r--r-- 1    0 1000 1221 Sep 19 18:21 backup.sh
```

#note It is important to note that if the `root_squash` option is set, we cannot edit the `backup.sh` file even as `root`.

## Unmounting

```sh
shakir0x00@htb[/htb]$ cd ..
shakir0x00@htb[/htb]$ sudo umount ./target-NFS
```
