### LINKS :
https://github.com/rebootuser/LinEnum
https://github.com/sleventyeleven/linuxprivchecker

https://github.com/411Hall/JAWS (windows)
https://github.com/peass-ng/PEASS-ng (BOTH)

## Vulnerable Software
Another thing we should look for is installed software. For example, we can use the `dpkg -l` command on Linux or look at `C:\Program Files` in Windows to see what software is installed on the system. We should look for public exploits for any installed software, especially if any older versions are in use, containing unpatched vulnerabilities.

## User Privileges

https://gtfobins.github.io/ (linux)
https://lolbas-project.github.io/# (windows)  Contains a list of Windows applications which we may be able to leverage to perform certain functions, like downloading files or executing commands in the context of a privileged user

```shell-session
shakir0x00@htb[/htb]$ sudo -l

    (user : user) NOPASSWD: /bin/echo
```

The `NOPASSWD` entry shows that the `/bin/echo` command can be executed without a password. This would be useful if we gained access to the server through a vulnerability and did not have the user's password. As it says `user`, we can run `sudo` as that user and not as root. To do so, we can specify the user with `-u user`:

```shell-session
shakir0x00@htb[/htb]$ sudo -u user /bin/echo Hello World!

    Hello World!
```

## Scheduled Tasks

. In Linux, a common form of maintaining scheduled tasks is through `Cron Jobs`. There are specific directories that we may be able to utilize to add new cron jobs if we have the `write` permissions over them. These include:

1. `/etc/crontab`
2. `/etc/cron.d`
3. `/var/spool/cron/crontabs/root`

## Exposed Credentials
 we can look for files we can read and see if they contain any exposed credentials. This is very common with `configuration` files, `log` files, and user history files (`bash_history` in Linux and `PSReadLine` in Windows)
 
#NOTE
 We may also check for `Password Reuse`, as the system user may have used their password for the databases


 ## SSH Keys
If we have read access over the `.ssh` directory for a specific user, we may read their private ssh keys found in `/home/user/.ssh/id_rsa` or `/root/.ssh/id_rsa`, and use it to log in to the server. If we can read the `/root/.ssh/` directory and can read the `id_rsa` file, we can copy it to our machine and use the `-i` flag to log in with it:

```shell-session
shakir0x00@htb[/htb]$ vim id_rsa
shakir0x00@htb[/htb]$ chmod 600 id_rsa
shakir0x00@htb[/htb]$ ssh root@10.10.10.10 -i id_rsa

root@10.10.10.10#
```


If we find ourselves with write access to a users`/.ssh/` directory, we can place our public key in the user's ssh directory at `/home/user/.ssh/authorized_keys`. This technique is usually used to gain ssh access after gaining a shell as that user. The current SSH configuration will not accept keys written by other users, so it will only work if we have already gained control over that user. We must first create a new key with `ssh-keygen` and the `-f` flag to specify the output file:

```shell-session
shakir0x00@htb[/htb]$ ssh-keygen -f key

Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase): *******
Enter same passphrase again: *******

Your identification has been saved in key
Your public key has been saved in key.pub
The key fingerprint is:
SHA256:...SNIP... user@parrot
The key's randomart image is:
+---[RSA 3072]----+
|   ..o.++.+      |
...SNIP...
|     . ..oo+.    |
+----[SHA256]-----+
```
This will give us two files: `key` (which we will use with `ssh -i`) and `key.pub`, which we will copy to the remote machine. Let us copy `key.pub`, then on the remote machine, we will add it into `/root/.ssh/authorized_keys`:

  Privilege Escalation

```shell-session
user@remotehost$ echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys
```

Now, the remote server should allow us to log in as that user by using our private key:

  Privilege Escalation

```shell-session
shakir0x00@htb[/htb]$ ssh root@10.10.10.10 -i key

root@remotehost# 
```