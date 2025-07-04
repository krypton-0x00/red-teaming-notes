## Permissions

The NTFS file system has many basic and advanced permissions. Some of the key permission types are:

| Permission Type      | Description                                                                                                                                                                                                                                                                                                                                                                                |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Full Control         | Allows reading, writing, changing, deleting of files/folders.                                                                                                                                                                                                                                                                                                                              |
| Modify               | Allows reading, writing, and deleting of files/folders.                                                                                                                                                                                                                                                                                                                                    |
| List Folder Contents | Allows for viewing and listing folders and subfolders as well as executing files. Folders only inherit this permission.                                                                                                                                                                                                                                                                    |
| Read and Execute     | Allows for viewing and listing files and subfolders as well as executing files. Files and folders inherit this permission.                                                                                                                                                                                                                                                                 |
| Write                | Allows for adding files to folders and subfolders and writing to a file.                                                                                                                                                                                                                                                                                                                   |
| Read                 | Allows for viewing and listing of folders and subfolders and viewing a file's contents.                                                                                                                                                                                                                                                                                                    |
| Traverse Folder      | This allows or denies the ability to move through folders to reach other files or folders. For example, a user may not have permission to list the directory contents or view files in the documents or web apps directory in this example c:\users\bsmith\documents\webapps\backups\backup_02042020.zip but with Traverse Folder permissions applied, they can access the backup archive. |

## Checking Perms from Command Line

```cmd
icacls c:\windows
```

- `(CI)`: container inherit
- `(OI)`: object inherit
- `(IO)`: inherit only
- `(NP)`: do not propagate inherit
- `(I)`: permission inherited from parent container

Basic access permissions are as follows:

- `F` : full access
- `D` :  delete access
- `N` :  no access
- `M` :  modify access
- `RX` :  read and execute access
- `R` :  read-only access
- `W` :  write-only access

## Modify Perms using Command Line
```cmd
icacls c:\Users /grant joe:f

icacls c:\users
c:\users WS01\joe:(F)
```

Removing 
```cmd
icacls c:\users /remove joe
```


# NTFS vs Share Permissions

Windows has **two types of permissions** for shared folders:
#### **Share Permissions** (When accessed _over the network_):

- **Full Control**: Can do everything (read, write, delete, and change permissions).
    
- **Change**: Can read, add, edit, and delete files.
    
- **Read**: Can only view files.
#### **NTFS Permissions** (Apply _locally and over the network_):

- **More granular and detailed** than share permissions.
    
- Examples:
    
    - **Modify**: Read, write, and delete.
        
    - **Read & Execute**: View and run files.
        
    - **Special Permissions**: Advanced settings like changing ownership or attributes.

> [!NOTE] 
**Important**: NTFS permissions always apply, whether you’re accessing files **locally or over the network**. Share permissions apply **only over SMB/network access**.

- In Windows, **NTFS permissions are inherited** by default (a subfolder inherits permissions from its parent).
- Admins can **disable inheritance** to set custom rules on specific folders.
- NTFS provides **finer control**, while share permissions are broader.

