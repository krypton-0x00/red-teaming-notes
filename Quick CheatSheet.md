# Subdomian Bruteforce VHOST.

```rb
gobuster vhost -u http://inlanefreight.htb:{PORT} -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain -t 200
```

# CRAWLING and SPIDERING

```rb
python3 ReconSpider.py http://{SUBDOMAIN}.inlanefreight.htb:{PORT}


cat results.json   
{  
    "emails": [],  
    "links": [],  
    "external_files": [],  
    "js_files": [],  
    "form_fields": [],  
    "images": [],  
    "videos": [],  
    "audio": [],  
    "comments": []  
}
```

