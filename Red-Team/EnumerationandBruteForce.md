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

From this screenshot, I can see some of the potential usernames we can use to login with, as well as domain allowing us to create sessions using blank usernames and passwords. Time to brute 
