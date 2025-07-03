## Scan default top 1000 ports
```sh
nmap -sV --open -oA nibbles_inital_scan <ipaddress>
```
- `-oA` output all scan formats -> includes XML, greppable and text output. 
- `--open` return only open ports

## Scan all ports
```sh
nmap -p- --open -oA nibbles_full_tcp_scan 10.129.42.190
```

## Script scan
```sh
nmap -sC -p 22,80 -oA nibbles_script_scan 10.129.42.190
```

## HTTP ENUM Script

```sh
nmap -sV --script=http-enum -oA nibbles_nmap_http_enum 10.129.42.190
```
- Can be used to enumerate common web application directories

## Simple TCP Connect Scan

Using Ping to identify the open ports

```sh
nmap -sT 10.129.108.229
```

## Host Discovery
```shell
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```


| `-sn` | Disables port scanning. |
| ----- | ----------------------- |

## Scan IP List
```shell
shakir0x00@htb[/htb]$ sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
```

| `-sn` | Disables port scanning.                                              |
| ----- | -------------------------------------------------------------------- |
| `-iL` | Performs defined scans against targets in provided 'hosts.lst' list. |

## Scan Multiple IPs

```shell
shakir0x00@htb[/htb]$ sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5

```

OR we can give range using 10.10.10.0-12

## Packet Trace 

```sh
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace 

```

| `-PE`            | Performs the ping scan by using 'ICMP Echo requests' against the target. |
| ---------------- | ------------------------------------------------------------------------ |
| `--packet-trace` | Shows all packets sent and received                                      |
| `--reason`       | Displays the reason for specific result.                                 |
we can also pass --reason to check my its marked as alive.
we can disable ARP pings by setting the "`--disable-arp-ping`" option.


## Host Discovery Strategies
| Ping Type         | Used When                  | Why?                                 |
| ----------------- | -------------------------- | ------------------------------------ |
| **ARP**           | Local network              | Reliable, works even if ICMP blocked |
| **ICMP (-PE)**    | Remote network (or forced) | Classic ping method                  |
| **TCP SYN (-PS)** | Optional                   | Useful when ICMP is blocked          |
| **TCP ACK (-PA)** | Optional                   | Detects firewalled but up hosts      |
## UDP Port Scan 


```shell
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.28 -F -sU
```


## Nmap - Specifying Scripts

```sh
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.28 -p 25 --script banner,smtp-commands
```

## Vulnerability Assessment

```sh
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.28 -p 80 -sV --script vuln 
```


# Performance
#### Optimized RTT

```sh
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms

```

#### Reduced Retries

```shell-session
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.0/24 -F --max-retries 0 | grep "/tcp" | wc -l

21
```

| `-F`                 | Scans top 100 ports.                                               |
| -------------------- | ------------------------------------------------------------------ |
| `--max-retries 0`    | Sets the number of retries that will be performed during the scan. |
## Rates

```sh
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.minrate300 --min-rate 300
```
#### Insane Scan

```sh
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.0/24 -F -oN tnet.T5 -T 5

```


# FIREWALLS

#### ACK-Scan
```sh
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.28 -p 21,22,25 -sA -Pn -n --disable-arp-ping --packet-trace

```

## Decoys

```sh
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5

```

| `-D RND:5` | Generates five random IP addresses that indicates the source IP the connection comes from. |
| ---------- | ------------------------------------------------------------------------------------------ |

## Scan by Using Different Source IP

```sh
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.28 -n -Pn -p 445 -O -S 10.129.2.200 -e tun0
```



| `-O`           | Performs operation system detection scan.              |
| -------------- | ------------------------------------------------------ |
| `-S`           | Scans the target by using different source IP address. |
| `10.129.2.200` | Specifies the source IP address.                       |
| `-e tun0`      | Sends all requests through the specified interface.    |
## DNS Proxying

## SYN-Scan of a Filtered Port

```sh
sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace
```

## SYN-Scan From DNS Port
```sh
shakir0x00@htb[/htb]$ sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53
```

## Connect To The Filtered Port

```sh
shakir0x00@htb[/htb]$ ncat -nv --source-port 53 10.129.2.28 50000

```

## Fragmentation

```sh
nmap -f 10.129.108.229
```


