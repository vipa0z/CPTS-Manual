`Port 111 2049`
configuration folder: `/etc/exports`
## Dangerous Settings

|**Option**|**Description**|
|---|---|
|`rw`|Read and write permissions.|
|`insecure`|Ports above 1024 will be used.|
|`nohide`|If another file system was mounted below an exported directory, this directory is exported by its own exports entry.|
|`no_root_squash`|All files created by root are kept with the UID/GID 0.|

nmap scripts:
```
--script nfs* -p111,2049
--script rpcinfo #included in sC flag
```

#### Show Available NFS Shares

```shell-session
showmount -e 10.129.14.128
```

#### Mounting NFS Share

```shell-session
mkdir target-NFS
sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock
cd target-NFS
tree .
.
└── mnt
    └── nfs
        ├── id_rsa
        ├── id_rsa.pub
        └── nfs.share

2 directories, 3 files
```

#### List Contents with Usernames & Group Names
```shell-session
$ ls -l mnt/nfs/
-rw-r--r-- 1 cry0l1t3 cry0l1t3  348 Sep 25 00:55 cry0l1t3.pub
-rw-r--r-- 1 root     root     1872 Sep 19 17:27 id_rsa
-
``` 
#### List Contents with UIDs & GUIDs

```shell-session
ls -n mnt/nfs/
-rw-r--r-- 1    0 1000 1221 Sep 19 18:21 backup.sh
```


```shell-session
ls -n mnt/nfs/
```

| **Command**                                               | **Description**                              |
| --------------------------------------------------------- | -------------------------------------------- |
| mkdir 'name' &&`showmount -e <FQDN/IP>`                   | Show available NFS shares.                   |
| `mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock` | Mount the specific NFS share to ./target-NFS |
| `umount ./target-NFS`                                     | Unmount the specific NFS share.              |
| `ls -l mnt/nfs/`                                          | List Contents with Usernames & Group Names   |
| <br>`ls -n mnt/nfs/`                                      | guid & uid                                   |

unmount 
```shell-session
sudo umount ./target-NFS
```
