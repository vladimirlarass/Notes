```
ffuf -u 'http://nocturnal.htb/view.php?username=FUZZ&file=*.pdf' -w
/usr/share/seclists/Usernames/xato-net-10-million-usernames.txt -mc 200 -fr
"User not found." -H "Cookie: PHPSESSID=4d9uqe8ifkefg1pkgu5ksi06qs"
```