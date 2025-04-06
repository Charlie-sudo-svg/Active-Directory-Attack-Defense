# Enumerating Active Directory

Although we already know the network layout, I first ran `sudo nmap -sn 10.0.2.1/24` to find all addresses that were live. Only Windows Server was running and not Windows 10 as seen below.

![Screenshot 2025-04-05 214346](https://github.com/user-attachments/assets/77901bd0-a500-4aed-bb82-dc6cdcfa2600)

In this scenario 10.0.2.15 is the Kali Linux IP and 10.0.2.10 is the Windows Server VM.

The next step I took was doing an nmap scan onto the Domain Controller using `sudo nmap 10.0.2.10`, super simple here as we are just looking for open ports.

![Screenshot 2025-04-05 214508](https://github.com/user-attachments/assets/2a110d94-55a9-4a76-a0e7-e3bf664089c6)

Wow! 13 different ports are open here! One port that looks interesting is port 445 also known as [Server Message Block](https://learn.microsoft.com/en-us/windows-server/storage/file-server/file-server-smb-overview)

I tried using anonymous SMB login with `smbclient -L //192.168.1.10 -N` but to no avail. Guess I shouldn't be so surprised anyways because I set up SMB on the Domain.

The next thing I tried is enum4linux. According to kali.org, "Enum4linux is a tool for enumerating information from Windows and Samba systems." I thought this would be a great starting point to try to enumerate for usernames, anything of the like. 

The command I used was `enum4linux -A 10.0.2.10` the -A flag runs enum4linux aggressively. I wanted to make sure no stone was left unturned. The results are posted
below:

![Screenshot 2025-04-05 215849](https://github.com/user-attachments/assets/5da80a4e-7a86-45c9-b0c4-7f3ea8437718)

From this screenshot, I can see some of the potential usernames we can use to login with, as well as domain allowing us to create sessions using blank usernames and passwords. Time to use the swiss army knife of penetration testing: crackmapexec.

"CrackMapExec, or CME, is a post-exploitation tool developed in Python and designed for penetration testing against networks. CrackMapExec collects Active Directory information to conduct lateral movement through targeted networks." 

(Source: https://attack.mitre.org/software/S0488/)

I ran the command `crackmapexec smb 10.0.2.10 -u 'administrator' -p passwords.txt` passwords.txt is a special file I made with a few passwords and one that was the actual right password only so I didn't have to wait around for brute force.

![Screenshot 2025-04-05 230128](https://github.com/user-attachments/assets/86d34f3e-7cc9-4d5a-8421-252472fbf1ce)

With the SMB password found, its time to login to a share. But which share exactly? I used `smbclient -L //10.0.2.10 -U administrator
` To figure that out.

![Screenshot 2025-04-05 230629](https://github.com/user-attachments/assets/f2dc14d9-5667-4ea7-b3f1-f074024a18ec)

It looks like the variable $C is the default share. Using `smbclient -U Administrator //10.0.2.10/$C` I was able to login to the share and gain access to it.


![Screenshot 2025-04-05 230814](https://github.com/user-attachments/assets/149d1bff-b534-43d7-aa48-c22cd6098a13)


Although there was nothing interesting when I when into the Users and Administrator directory, in a real world organization this could cause devastation among other things. Sensitive information, passwords, and databases could all be stored on here or within reach. 
