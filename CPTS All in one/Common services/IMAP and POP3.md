IMAP: 143,993
POP3: 110,995


## IMAP Commands

| **Command**                     | **Description**                                                                                               |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `1 LOGIN username password`     | User's login.                                                                                                 |
| `1 LIST "" *`                   | Lists all directories.                                                                                        |
| `1 CREATE "INBOX"`              | Creates a mailbox with a specified name.                                                                      |
| `1 DELETE "INBOX"`              | Deletes a mailbox.                                                                                            |
| `1 RENAME "ToRead" "Important"` | Renames a mailbox.                                                                                            |
| `1 LSUB "" *`                   | Returns a subset of names from the set of names that the User has declared as being `active` or `subscribed`. |
| `1 SELECT INBOX`                | Selects a mailbox so that messages in the mailbox can be accessed.                                            |
| `1 UNSELECT INBOX`              | Exits the selected mailbox.                                                                                   |
| `1 FETCH <ID> all`              | Retrieves data associated with a message in the mailbox.                                                      |
| `1 CLOSE`                       | Removes all messages with the `Deleted` flag set.                                                             |
| `1 LOGOUT`                      | Closes the connection with the IMAP server.                                                                   |

## POP3 Commands

|**Command**|**Description**|
|---|---|
|`USER username`|Identifies the user.|
|`PASS password`|Authentication of the user using its password.|
|`STAT`|Requests the number of saved emails from the server.|
|`LIST`|Requests from the server the number and size of all emails.|
|`RETR id`|Requests the server to deliver the requested email by ID.|
|`DELE id`|Requests the server to delete the requested email by ID.|
|`CAPA`|Requests the server to display the server capabilities.|
|`RSET`|Requests the server to reset the transmitted information.|
|`QUIT`|Closes the connection with the POP3 server.|

## CURL

IMAP / POP3

```shell-session
shakir0x00@htb[/htb]$ curl -k 'imaps://10.129.14.128' --user cry0l1t3:1234 -v

```

## OpenSSL - TLS Encrypted Interaction POP3

```sh
openssl s_client -connect 10.129.14.128:pop3s

```

## OpenSSL - TLS Encrypted Interaction IMAP

```sh
openssl s_client -connect 10.129.14.128:imaps
```



# OPTIONAL 

After connection to IMAP server ( Approach )

1. `1 LIST "" *`  -> List available mail boxes 
		```
		DEV
		DEV.DEPARTMENT
		DEV.DEPARTMENT.INT
		INBOX
		```
2. `2 SELECT INBOX` Selects the INBOX one or we can select DEV.DEPT.INT
3. `3 SEARCH ALL` -> Check how many messages exists.
4. `4 FETCH 1 RFC822` -> Fetch the contents of the email 


