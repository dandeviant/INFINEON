
### Secure Shell (ssh)
> - An alternative for the trashy ass Telnet protocol for better data security and better authentication
> - Universally uses port 22/tcp

Basic syntax;
```bash
ssh [username]@[IPv4]
```

#### Sub of SSH: ssh-keygen
> [! ]- What is ssh-keygen?
> - An SSH tool that generates SSH `id_rsa` keys used during first time SSH authentication

```
ssh-keygen [options] [hostname]
```

> Sample:
> ```
> daniel@ubuntu:~/$ ssh-keygen -R sa-my-mkz-8cp-0501
> ```


---
### Secure Copy Protocol (scp)

> - An SSH-driven protocol for secure file transfer via SSH lines
> - Uses methods similar to SSH in terms of authentication (username, password, also needs `id_rsa` key if target is configured as so)


- To simplify the syntax;
```bash
scp [source] [destination]
```


| Commands | Sample |
| --- | --- |
| `scp [source directory] [username]@[target IPv4]:[destination file full path]` | scp local files to remote destination
| `scp [username]@[target IPv4]:[source file full path] [destination path]` | scp remote file to local destination
| `scp [username]@[target IPv4]:[target file full path] [username]@[target IPv4]:[destination file full path] ` | scp remote file to remote destination
