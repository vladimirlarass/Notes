`Network File System`( `NFS`) est un système de fichiers réseau développé par Sun Microsystems et dont l'objectif est le même que SMB.

```
sudo nmap --script nfs* $ip$ -sV -p111,2049
```

Afficher les partage NFS Disponible 

```
showmount -e 10.129.14.128
```

Montage du partage NFS 

```
mkdir target-NFS
sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock
cd target-NFS
tree .
```