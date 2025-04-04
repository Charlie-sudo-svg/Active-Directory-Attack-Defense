# Active Directory Setup

I downloaded a Windows Server 2022 image onto a VM and immediately got started with server manager. 
To set up Active Directory, click manage -> add roles and features then select Active Directory Domain Services. 

![Screenshot 2025-03-29 201642](https://github.com/user-attachments/assets/779cc9fc-bce8-4db6-91cb-fd005ef65d6f)

After clicking through all of the options, promote the server to a domain controller then add a new forest. Once AD is installed, Windows Server should restart.

![Screenshot 2025-03-29 202609](https://github.com/user-attachments/assets/cd8801a0-9b8a-4256-b49f-faf738780f67)

Now that Active Directory is officially set up, we can add our windows 11 machine to the directory.
