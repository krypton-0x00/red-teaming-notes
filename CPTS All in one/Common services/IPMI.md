[Intelligent Platform Management Interface](https://www.thomas-krenn.com/en/wiki/IPMI_Basics) (`IPMI`) is a set of standardized specifications for hardware-based host management systems used for system management and monitoring. It acts as an autonomous subsystem and works independently of the host's BIOS, CPU, firmware, and underlying operating system.

```sh
sudo nmap -sU --script ipmi-version -p 623 ilo.inlanfreight.local
```


## Metasploit Version Scan

```
 use auxiliary/scanner/ipmi/ipmi_version 

```

#tip: During internal penetration tests, we often find BMCs where the administrators have not changed the default password. Some unique default passwords to keep in our cheatsheets include:


| Product         | Username      | Password                                                                  |
| --------------- | ------------- | ------------------------------------------------------------------------- |
| Dell iDRAC      | root          | calvin                                                                    |
| HP iLO          | Administrator | randomized 8-character string consisting of numbers and uppercase letters |
| Supermicro IPMI | ADMIN         | ADMIN                                                                     |

## Metasploit Dumping Hashes

```
use auxiliary/scanner/ipmi/ipmi_dumphashes 
```
