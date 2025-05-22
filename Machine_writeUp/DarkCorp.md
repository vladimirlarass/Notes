## enumeration
# Nmap

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 33:41:ed:0a:a5:1a:86:d0:cc:2a:a6:2b:8d:8d:b2:ad (ECDSA)
|_  256 04:ad:7e:ba:11:0e:e0:fb:d0:80:d3:24:c2:3e:2c:c5 (ED25519)
80/tcp open  http    nginx 1.22.1
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: nginx/1.22.1
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
 
 Nous avons deux port ouvert 22 (SSH) et 80 (HTTP)
 apres avoir visiter le site web nous devons ajouter le domaine 
 
```bash
 echo "10.10.11.54 drip.htb" | sudo tee -a /etc/hosts > /dev/null
```

![[Pasted image 20250522182528.png]]

Nous tombons sur le site internet essayons de creer un compte et nous connecter 

![[Pasted image 20250522182632.png]]

Maintenant je vais essayer de regarder le code de l'email 

![[Pasted image 20250522182322.png]]

Nous avons trouve un autre domaine 

```bash
echo "10.10.11.54 drip.darkcorp.htb" | sudo tee -a /etc/hosts > /dev/null
```

