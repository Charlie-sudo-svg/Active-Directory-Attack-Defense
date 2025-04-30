# PFsense Setup With Snort Integration

When I last left off in [/Setup/PFsense.md](../Setup/PFsense.md) I had gotten PFsense to work but I hadn't yet logged in and configured anything -- Let's change that. 

The first thing I did when I logged in is I changed to dark mode immediately by going to system -> general setup, then started the real work.

## Step 1: Changing default credentials 

This one shouldn't even have to be said, but many organizations are probably still running some important services with default credentials... Not me though!

The default credentials for PFsense are admin/pfsense so to change that I went to System -> User Manager and edited the password of the Admin account to something super sneaky.

![Screenshot 2025-04-29 175707](https://github.com/user-attachments/assets/8ce01a98-f383-43c6-9f1b-6611ff2f4c25)

### Why is this a good password?

This is a great password security wise because it includes random characters all throughout the password (not to mention its 25 character long.) In fact, this password is so secure it'll probably be cracked after the heat death of the universe.

![Untitled design](https://github.com/user-attachments/assets/f826d9f8-30e3-4fe0-a5d3-41e5505b04b3)

## Step 2: Firewall Rules

As I established in the network graph in /Setup, I added another interface to the PFsense firewall to account for a real world scenario in which an attacker from another network can attack us. The IP address of the network is 72.146.234.1/24, this is the range of IPs that the attacker (kali linux machine) will have. The goal for adding the firewall rule is to block this attacker from being able to reach the enterprise in any manner.

The first thing I had to do was set up the firewall rule for OPT1, this is the Kali attackers network. 

![Screenshot 2025-04-29 190702](https://github.com/user-attachments/assets/682fa352-c61a-4de8-af20-763bb64a6c32)

The rule just outlines that any type of traffic from OPT1 can come through the firewall. Just pretend that OPT1 is regular internet traffic, because I am using VM's its a little harder to actually simulate regular network traffic. As far as I'm concered, OPT1 is just the internet, and people access the internet all the time, including our attacker. 

Now that the attacker than ping the firewall it can also ping the machines inside the firewall, which can lead to attacks if not secured. 

## Step 3: Installing Snort

The next step is to install Snort to alert me of any suspicious activity happening on the network. If I was an analyst, I'd have no idea an attack was happening unless I did some really deep digging or I got an alert. Once we set up Snort and get an alert, we can properly take action by blocking the IP over the firewall.
