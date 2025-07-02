PORTS: 25, 587, 465

## SMTP Commands

|Command|Purpose|
|---|---|
|`HELO`|Start the conversation (old command)|
|`EHLO`|Enhanced version of HELO (modern, preferred)|
|`MAIL FROM:`|Tells the server who the sender is|
|`RCPT TO:`|Tells the server who the recipient is|
|`DATA`|Starts writing the email content|
|`AUTH`|Used to authenticate (login with username/password)|
|`QUIT`|Ends the connection|
|`VRFY`|Ask if a user exists (not always reliable)|

## Connection Using Telnet

```sh
telnet 10.129.14.128 25

```


## **VRFY Command (User Enumeration)**

You can try to see if a user exists with:

```bash
VRFY root
```

But if the server is misconfigured, it will always return "OK" — even for fake users — like:

```bash
VRFY aaaaaaaaaaaaaaaaaaa
252 2.0.0 aaaaaaaaaaaaaaaaaaa
```

So you **can’t fully trust it**.

## **Using a Web Proxy**

If you are behind a proxy (e.g., in a corporate or restricted network), you can still access SMTP like this:

```http
CONNECT 10.129.14.128:25 HTTP/1.0
```

This makes the proxy open a raw connection to the SMTP server on your behalf.

## SMTP USERNAME BRUTEFORCE

```sh
smtp-user-enum -M VRFY -U /usr/share/seclists/Usernames/top-usernames-shortlist.txt -t 10.129.234.150
```



## Open Relay Configuration

SMTP

```shell-session
mynetworks = 0.0.0.0/0
```

With this setting, this SMTP server can send fake emails and thus initialize communication between multiple parties. Another attack possibility would be to spoof the email and read it.

## Nmap

```sh
sudo nmap 10.129.14.128 -sC -sV -p25

```

## Nmap - Open Relay

```sh
sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v

```
