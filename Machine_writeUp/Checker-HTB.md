# Footrprinting

```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 aa:54:07:41:98:b8:11:b0:78:45:f1:ca:8c:5a:94:2e (ECDSA)
|_  256 8f:2b:f3:22:1e:74:3b:ee:8b:40:17:6c:6c:b1:93:9c (ED25519)
80/tcp   open  http    Apache httpd
|_http-server-header: Apache
|_http-title: 403 Forbidden
8080/tcp open  http    Apache httpd
|_http-server-header: Apache
|_http-title: 403 Forbidden
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

j'ai trouve 3 ports ouverts 
port 80
port 22
port 8080 

## Web Application Discovery
je suis tombe sur la page web , qui est un une page de login . j'ai essaye des emails et mots de passe basique rien . 

![[Pasted image 20250226110719.png]] 
<img src="20250226110719.png" > 

Apres j'ai intercepte dans burpsuite la page web j'ai trouve un cookie 

```
eyJpdiI6IjlDbThXbUc2QmUxRDBRWUE3bzhHWVE9PSIsInZhbHVlIjoibVdYWUpCM0pRQTVyd3VMZlNmaHpOeFdCNUFKNlhBcTlRd0xWVnV2dzNkS1FYUGZzL3czV0VTMlhkUXZIZmNRT3BIdFZxWGRhclNLV0FCMnJXaUhTYTJhaThrU3hpazhSNjVYZDdKYVl3RHBpTkdqOEFyNUxQMVpaY2VwbzkvSUgiLCJtYWMiOiIyZTQ0NWVlNThkYmJkNGM5ZDdhZTYwNmQ4MDQwYzllNzBjZDJjNzM3NmFiNGJkZTM0Y2ZkZGVkNTA3Zjg0YTAyIiwidGFnIjoiIn0%3D;
```



![[Pasted image 20250226110631.png]] 

