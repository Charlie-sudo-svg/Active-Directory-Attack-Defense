# Active Directory Misconfigurations

For the attack to work, I need to misconfigure a few settings within active directory and the network. 

The first thing I need to do is enable LDAP anonymous binds. This will allow for tools like enum4linux and ldapsearch to map Active Directory without credentials. I did this by opening adsiedit.msc and creating a new configuration. Under "Connection Point" I selected, "Select a well known Naming Contect" and clicked Configuration. 


After creating the configuration I went into the Services -> Windows NT -> Directory Services folder. I right clicked on the folder and selected properties. After I selected properties I changed dSHeuristics value to 0000002. By doing this, I enabled anonymous access for anyone. 

![Screenshot 2025-05-11 214307](https://github.com/user-attachments/assets/fddddb34-d0c1-4e03-b3d4-fefd6cf9dbd3)


### Step 2: Changing ACLs to allow anonymous users to read directory data

I went to Active Directory Users and Computers and selected View -> Advanced Features. After that, I right clicked the domain root and selected properties. From here, I went to security and added ANONYMOUS USERS and allowed read permissions.

![Screenshot 2025-05-11 215604](https://github.com/user-attachments/assets/7d3f78d3-881a-4984-85f6-299aa59de2ba)

By doing this, I'm allowing tools like ldapsearch, crackmapexec, and bloodhound to work without credentials (Which is very unsafe but is the whole point!)

## Opening an SMB Share

To open an SMB share, I navigated to the Server Manager and clicked on File and Storage Services on the lefthand side of the GUI. From there, I navigated to shares and created a new one called, "Business Stuff."

![image](https://github.com/user-attachments/assets/22374ba4-db55-4b6a-874e-f1719a4be9de)

After clicking through the wizard I came across permissions. In the permissions section I added everyone FULL access to the share, this will be useful later on.

![image](https://github.com/user-attachments/assets/4515f5fd-82a9-43d7-a102-d641cbe2a9fb)


I went to my local security policy and changed these settings: 

Network Access: Let everyone permissions apply to anonymous users: Enabled

Network Access: Shares that can be accessed anonymously: BusinessStuff

Network Access: Do not allow anonymous enumeration of SAM accounts and shares: Disabled

After setting those policies I ran `gpupdate /force` in command prompt to force save the changes.

Last but not least, I enabled SMBv1 to really misconfigure this server.

I used  `Enable-WindowsOptionalFeature -Online -FeatureName "SMB1Protocol" -All` to enable smbv1. This should be all we need to start attacking the machine!
