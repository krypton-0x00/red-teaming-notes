
### 🛡️ What is SSH?

* SSH is a **secure protocol** used to connect two computers over a network.
* It ensures **encryption**, meaning no one else can read the data even if they intercept it.
* SSH works on **port 22** by default and is available on **Linux, macOS**, and even **Windows** (with tools like OpenSSH).

---

### 📜 SSH Protocol Versions

* **SSH-1**: Old, outdated, and vulnerable to attacks like **Man-in-the-Middle (MITM)**.
* **SSH-2**: Improved version—**more secure**, **faster**, and **more stable**. It’s the current standard.

---

### 💻 What Can SSH Be Used For?

* Remotely controlling another machine (via command-line or GUI).
* Running commands.
* Transferring files.
* Forwarding ports (useful in secure tunneling).

---

### 🔐 Authentication in SSH

SSH supports several authentication methods. The one explained in detail is:

### ✅ Public Key Authentication

**How it works:**

1. **You (the client)** create a **key pair**:

   * **Private key**: Stays securely on your computer.
   * **Public key**: Stored on the SSH **server**.

2. When you connect:

   * The **server** checks your identity using the **public key**.
   * It sends a challenge that can only be solved by someone with the **matching private key**.
   * You solve it using your private key and send back the answer.

3. If the answer is correct, you're allowed to connect.

4. To protect the private key, it's encrypted with a **passphrase**—a longer, more secure version of a password.

5. Once you enter your passphrase and unlock your private key, you can connect to **multiple servers** without re-entering passwords—until you log out.

---

### 🧠 Key Advantages of Public Key Authentication

* **More secure** than passwords.
* **Prevents brute-force attacks** and **keylogging** issues.
* **Convenient**: Enter the passphrase only once per session.
* **No password sent over the network**.
