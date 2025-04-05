# Setting up VMs for Attacks

The first thing I did was download Kali Linux from kali.org. 

The setup from here is pretty simple. I enabled remote protocols and opened ports on the firewall of the Windows 10 and Windows Server machine to allow communication between the server and Windows 10,
aswell as enumeration from the Linux machine.

The commands I ran are below:

`Enable-PSRemoting -Force`

`Set-NetFirewallRule -DisplayGroup "Windows Remote Management" -Enabled True
Set-NetFirewallRule -DisplayGroup "File and Printer Sharing" -Enabled True`

The first command, enables Powershell Remoting on the Domain Controller and Windows 10. Powershell Remoting allows users to run powershell commands remotely, this 
is commonly used in organizations so IT departments could fix issues remotely.

The second command, allows SMB, WMI, RPC, and WinRM through the firewall which is essential to setting up SMB shares across an organization aswell as using Bloodhound for 
enumeration later.
