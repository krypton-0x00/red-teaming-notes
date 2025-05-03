### 📁 Default `sshd_config` Settings

The command used:

```bash
cat /etc/ssh/sshd_config | grep -v "#" | sed -r '/^\s*$/d'
```

filters out comments and empty lines to show only **active settings** in the SSH server config. Example output:

```
Include /etc/ssh/sshd_config.d/*.conf
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding yes
PrintMotd no
AcceptEnv LANG LC_*
Subsystem sftp /usr/lib/openssh/sftp-server
```

This shows that most settings are **not explicitly defined**, so SSH uses default behaviors—some of which can be risky.

---

### 🚨 Dangerous Settings to Watch Out For

|**Setting**|**Risk**|
|---|---|
|`PasswordAuthentication yes`|Allows brute-force attacks if usernames are known.|
|`PermitEmptyPasswords yes`|Allows login with no password—**huge security hole**.|
|`PermitRootLogin yes`|Logging in directly as root gives attackers **full control** if cracked.|
|`Protocol 1`|Uses outdated, vulnerable encryption. **Use SSH-2 only.**|
|`X11Forwarding yes`|Enables GUI forwarding, which had **command injection vulnerabilities**.|
|`AllowTcpForwarding yes`|Can be abused for tunneling and hiding attacker activity.|
|`PermitTunnel`|If enabled, may allow creation of network tunnels—**can be abused**.|
|`DebianBanner yes`|Reveals system info, potentially helping fingerprinting attacks.|

---

### 🧠 Why These Are Dangerous

- **Humans create predictable passwords**, often with minor tweaks like adding `123`, `!`, or capitalizing the first letter.
    
- Tools like **Hydra**, **Medusa**, or **John the Ripper** can guess passwords using smart mutation rules.
    
- **Allowing root login + password authentication = serious threat**, especially if the root password is weak.
    

---

### 🛡️ Hardening Recommendations

To secure your SSH server:

- Disable password auth:
    
    ```bash
    PasswordAuthentication no
    ```
    
- Disallow root login:
    
    ```bash
    PermitRootLogin no
    ```
    
- Only use SSH-2:
    
    ```bash
    Protocol 2
    ```
    
- Disable X11 forwarding if GUI is not needed:
    
    ```bash
    X11Forwarding no
    ```
    
- Use public key authentication only.
    
- Enforce strong passphrases for private keys.
    
- Use **fail2ban** or **DenyHosts** to block brute-force attempts.
    
- Keep OpenSSH updated.
    
