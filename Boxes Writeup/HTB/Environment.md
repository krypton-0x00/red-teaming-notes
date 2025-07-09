## Ports

80 -> Website environment.htb
22 -> SSH 


## Webserver Recon

1. Nginx/1.22.1

##### Routes

```sh
200      GET        1l       27w     1713c http://environment.htb/build/assets/styles-Bl2K3jyg.css
200      GET       54l      174w     2391c http://environment.htb/login


405      GET        0l        0w        0c http://environment.htb/mailing 


200      GET       87l      392w     4602c http://environment.htb/
200      GET        1l      119w     4111c http://environment.htb/build/assets/login-CnECh1Us.css
200      GET       50l      135w     2126c http://environment.htb/up


302      GET       12l       22w      358c http://environment.htb/logout => http://environment.htb/login
301      GET        7l       11w      169c http://environment.htb/storage => http://environment.htb/storage/
301      GET        7l       11w      169c http://environment.htb/storage/files => http://environment.htb/storage/files/
301      GET        7l       11w      169c http://environment.htb/build => http://environment.htb/build/
301      GET        7l       11w      169c http://environment.htb/vendor => http://environment.htb/vendor/
301      GET        7l       11w      169c http://environment.htb/build/assets => http://environment.htb/build/assets/

```

405 Reveled the Larval and PHP version

![[Pasted image 20250709115007.png]]
![[Pasted image 20250709115020.png]]

![[Pasted image 20250709115740.png]]
