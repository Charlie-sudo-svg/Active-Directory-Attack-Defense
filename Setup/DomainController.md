# Active Directory Setup

I downloaded a Windows Server 2022 image onto a VM and immediately got started with server manager. 
To set up Active Directory, click manage -> add roles and features then select Active Directory Domain Services. 

![Screenshot 2025-03-29 201642](https://github.com/user-attachments/assets/779cc9fc-bce8-4db6-91cb-fd005ef65d6f)

After clicking through all of the options, promote the server to a domain controller then add a new forest. Once AD is installed, Windows Server should restart.

![Screenshot 2025-03-29 202609](https://github.com/user-attachments/assets/cd8801a0-9b8a-4256-b49f-faf738780f67)

Now that Active Directory is officially set up, we can add our windows 11 machine to the directory.

## Adding Users

The next step is to add a user. For this project, only one user will be added to the domain but more may be added in the future. To add a user, I went to server manager and clicked 'tools', then 'Active Directory Users and Computers.' After navigating to the domain I created a new OU (Organizational Unit) named Executives.
From there, I created a user called Arthur Morris with the username 'ArthurM' and password 'p@SSw0Rd'

![Screenshot 2025-04-04 201130](https://github.com/user-attachments/assets/2ef5980d-b6ed-4c25-8fb0-deaea49240e6)

## Setting up DNS

Now that the users are added thats great! The only issue is, my user can't access the internet due to a DNS issue. This happens because when the Windows 10 machine connects to the domain, the DNS server on the machine has to be that of the Windows Server. To fix this, I set up DNS Forwarding on the Windows Server. 

I typed in Win + R and dnsmgmt.msc to open up the DNS manager. After this, I right clicked my servers name and clicked on properties.

The next step is to go to the Forwarders tab and add `8.8.8.8` as one of the forwarders. This fixes the DNS issue I had on the Windows 10 machine.

![Screenshot 2025-04-04 202340](https://github.com/user-attachments/assets/d63a2f09-d952-40f6-b00d-df01fc9d9ff7)
