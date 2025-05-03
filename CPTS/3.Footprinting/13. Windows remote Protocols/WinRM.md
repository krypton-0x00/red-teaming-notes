**WinRM** is a built-in **remote management protocol** in Windows that allows you to **control and manage remote computers** using the command line.

It's especially useful for:

* Running scripts or commands on remote systems
* Managing computers without needing a full GUI (graphical user interface)
* Automating administrative tasks across multiple systems

---

### **How does it work?**

WinRM works using a **protocol called SOAP (Simple Object Access Protocol)**, which is a way for programs to talk to each other over a network using standard messages.

* It communicates over **TCP ports 5985 (HTTP)** and **5986 (HTTPS)**.

  * Port **5986** is more secure because it uses **HTTPS encryption**.
  * Older ports like **80 (HTTP)** and **443 (HTTPS)** were used in the past, but they’re avoided now due to security concerns.

---

### **What is WinRS?**

**WinRS (Windows Remote Shell)** is a tool that works with WinRM and allows you to **run any command** on a remote system.

* For example, you could run:

  ```
  winrs -r:RemotePCName ipconfig
  ```

  to check the IP configuration of a remote computer.

---

### **When is WinRM available?**

* WinRM is **enabled by default** in **Windows Server 2012 and newer**.
* In **older systems or Windows clients (like Windows 7/10)**, you must **manually enable and configure it**.
* You also need to **set up firewall rules** to allow connections through the required ports.

---

### **What is it used for?**

WinRM is used by many Windows tools, especially:

* **PowerShell Remoting** (running PowerShell commands remotely)
* **Event log collection and merging** from remote machines
* Centralized system management


## Footprinting the Service

As we already know, WinRM uses TCP ports `5985` (`HTTP`) and `5986` (`HTTPS`) by default, which we can scan using Nmap. However, often we will see that only HTTP (`TCP 5985`) is used instead of HTTPS (`TCP 5986`).

#### Nmap WinRM

Windows Remote Management Protocols

```shell-session
shakir0x00@htb[/htb]$ nmap -sV -sC 10.129.201.248 -p5985,5986 --disable-arp-ping -n

Starting Nmap 7.92 ( https://nmap.org ) at 2021-11-06 16:31 CET
Nmap scan report for 10.129.201.248
Host is up (0.030s latency).

PORT     STATE SERVICE VERSION
5985/tcp open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.34 seconds
```

If we want to find out whether one or more remote servers can be reached via WinRM, we can easily do this with the help of PowerShell. The [Test-WsMan](https://docs.microsoft.com/en-us/powershell/module/microsoft.wsman.management/test-wsman?view=powershell-7.2) cmdlet is responsible for this, and the host's name in question is passed to it. In Linux-based environments, we can use the tool called [evil-winrm](https://github.com/Hackplayers/evil-winrm), another penetration testing tool designed to interact with WinRM.

Windows Remote Management Protocols

```shell-session
shakir0x00@htb[/htb]$ evil-winrm -i 10.129.201.248 -u Cry0l1t3 -p P455w0rD!

Evil-WinRM shell v3.3

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM Github: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\Cry0l1t3\Documents>
```