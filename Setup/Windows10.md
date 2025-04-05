# Windows 10 Machine

The first thing I wanted to do was connect Windows 10 to the Active Directory domain. 

To join our Windows 10 machine to the domain I set up a manual ip to 10.0.2.5, then I went into settings and found the “Access work or school” tab. 
After clicking onto that, I clicked “Join this device to a local Active Directory domain.” From there I entered my domain name and credentials. 

Note, to actually do this you have to set your preferred DNS server to the IP of the Windows Server 2022 VM.

![Screenshot 2025-04-04 194723](https://github.com/user-attachments/assets/d163e393-c7b4-4586-bb0d-4dedcc8ea64b)

After joining to the domain, the computer restarts and we need to login as the user 'Arthur' we made as the Domain Controller.
