
### 🕹️ What Are R-Services?

* Developed by **CSRG at UC Berkeley**, these were early remote-access tools for Unix systems.
* Communicate **unencrypted** over the network (like Telnet).
* Replaced today by **SSH**, which provides **encryption** and **secure authentication**.

---

### 🔓 Why R-Services Are Dangerous

* **No encryption**: All data, including usernames and passwords, is sent in **plaintext**.
* Vulnerable to:

  * **Man-in-the-middle (MITM) attacks**
  * **Credential sniffing**
* **Trust-based access** via files like:

  * `/etc/hosts.equiv`
  * `~/.rhosts`
* If a host is listed in these files, **no password is required** to log in from that host as a specific user. This trust model is easily abused.

---

### 🔧 Key R-Commands and Services

| **Command** | **Daemon** | **Port** | **Description**                                                                       |
| ----------- | ---------- | -------- | ------------------------------------------------------------------------------------- |
| `rcp`       | `rshd`     | 514/TCP  | Copies files between systems (like `cp`) but **without warning** on overwrites.       |
| `rsh`       | `rshd`     | 514/TCP  | Opens a remote shell **without login prompts**—uses `.rhosts` and `/etc/hosts.equiv`. |
| `rexec`     | `rexecd`   | 512/TCP  | Executes remote commands **with plaintext username/password**.                        |
| `rlogin`    | `rlogind`  | 513/TCP  | Logs into a remote machine like `telnet`, but Unix-only.                              |

---

### 📁 Example: `/etc/hosts.equiv`

This file sets up **trusted hosts**. If a host and username are listed, that user can connect **without authentication**.

Example:

```rb
shakir0x00@htb[/htb]$ cat .rhosts

htb-student     10.0.17.5
+               10.0.17.10
+               +
```

As we can see from this example, both files follow the specific syntax of `<username> <ip address>` or `<username> <hostname>` pairs. Additionally, the `+` modifier can be used within these files as a wildcard to specify anything. In this example, the `+` modifier allows any external user to access r-commands from the `htb-student` user account via the host with the IP address `10.0.17.10`.
 
---

### ⚔️ Pentesting Tip

During internal tests, finding these files (`/etc/hosts.equiv`, `~/.rhosts`) with permissive entries is a **high-value discovery**. Why?

* You can **bypass authentication** and gain **shell access** using `rsh`.
* If an attacker compromises one system, they can often move **laterally** to others **without needing credentials**.

---

### 🛡️ Real-World Defense

* **Disable R-Services** on all systems.
* Use **SSH** instead.
* Remove any `hosts.equiv` or `.rhosts` files if they exist.
* If R-Services *must* be used (legacy reasons), isolate the systems and **enforce strict firewall rules**.
