# PFsense Installation

The first step to setting up PFsense was to get the networking right. For this project, I am using Virtualbox for all of my VM'ing needs. I set up a virtual machine with the PFsense ISO and a bridged adapter for the 1st adapter and on the second adapter I connected it to the NAT network. The NAT network has an ip of 10.0.2.0/24.

When you first launch a new PFsense ISO this should be the first screen you get: 

![Screenshot 2025-04-28 172110](https://github.com/user-attachments/assets/639f85fb-bf50-4813-be97-502c0e953cb1)

To proceed, I clicked enter until I got to a screen that asked me to configure the WAN interface. This is important because I have 2 network adapters set up, the first one is a bridged adapter that looks out to the internet, and the 2nd one is the NAT network which looks into the LAN of the "organization."

![Screenshot 2025-04-28 172214](https://github.com/user-attachments/assets/2c78f26b-d8a7-408b-bf10-8a191bce6f22)

I selected em0 as the wan interface. After that, I clicked enter a few more times until I got to configuring the LAN interface. This is also pretty simple, remember that the 2nd network adapter looks into the local network, therefore I selected em1 as the LAN interface as seen below.

![Screenshot 2025-04-28 172228](https://github.com/user-attachments/assets/75d6158a-0839-48f9-a5e8-8feed900b37a)

After this, I clicked through all the other configurations and eventually got to the point where PFsense was actually installing. Just as a quick note, I installed using version 2.7.2 which is the current stable release at the time of writing.

When the installation was completed, I rebooted the system and removed the ISO disk from virtualbox so PFsense would boot from the hard drive. After booting PFsense I got a screen that looked like this:

![Screenshot 2025-04-28 173953](https://github.com/user-attachments/assets/e2d52292-7864-464f-a143-c77cf1ca9238)

The last step is fairly simple, I typed in 2 into the PFsense interface to change the IPv4 address of the LAN to 10.0.2.1 so I could access PFsense from 10.0.2.1 on any computer on the network. After clicking through the options and setting the IP address, I was finally able to get to this screen on the Windows Server machime:

![Screenshot 2025-04-28 175038](https://github.com/user-attachments/assets/ae92cd18-33ef-41bb-b177-8ddc5c317654)


