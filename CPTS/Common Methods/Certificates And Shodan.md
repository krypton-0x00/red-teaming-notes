## Certificate Transparency Log
**crt.sh** is a website that lets you search all certificates issued


```sh
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq .
```

## Company Hosted Servers
You want to know which subdomains are actually hosted **by the company** (not by AWS or other third parties)

```sh
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done
```

## Shodan - IP List

```sh
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f4 >> ip-addresses.txt;done
```
then
```sh
for i in $(cat ip-addresses.txt);do shodan host $i;done

```

## DNS Records
```sh
dig any inlanefreight.com
```
