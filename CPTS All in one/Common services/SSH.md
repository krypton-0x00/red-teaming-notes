## Fingerprint the SSH server
```sh
./ssh-audit.py 10.129.14.132
```

we can specify the authentication method with the SSH client option `PreferredAuthentications`.
```sh
ssh -v cry0l1t3@10.129.14.132 -o PreferredAuthentications=password
```

