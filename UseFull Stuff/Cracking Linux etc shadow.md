
```sh
~/test 
❯ nvim shadow.txt      //get the shadowfile from the box                          

~/test 
❯ nvim passwd.txt     // get the /etc/passwd file

~/test 
❯ unshadow passwd.txt shadow.txt > passwords.txt

~/test 
❯ john --wordlist=/usr/share/seclists/Passwords/Common-Credentials/100k-most-used-passwords-NCSC.txt passwords.txt

~/test 
❯ 
```