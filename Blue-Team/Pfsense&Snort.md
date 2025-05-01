# PFsense Setup With Snort Integration

When I last left off in [/Setup/PFsense.md](../Setup/PFsense.md) I had gotten PFsense to work but I hadn't yet logged in and configured anything -- Let's change that. 

The first thing I did when I logged in is I changed to dark mode immediately by going to system -> general setup, then started the real work.

## Step 1: Changing default credentials 

Before touching any firewall rules or packages, I did what every security-conscious admin should do: change the default credentials.

PFsense ships with admin:pfsense as its default login. To change this:

Go to System → User Manager

Edit the admin account and update the password to something strong

![Screenshot 2025-04-29 175707](https://github.com/user-attachments/assets/8ce01a98-f383-43c6-9f1b-6611ff2f4c25)

### Why is this a good password?

This is a great password security wise because it includes random characters all throughout the password (not to mention its 25 character long.) In fact, this password is so secure it'll probably be cracked after the heat death of the universe.

![Untitled design](https://github.com/user-attachments/assets/f826d9f8-30e3-4fe0-a5d3-41e5505b04b3)

## Step 2: Firewall Rules

As I established in the network graph in /Setup, I added another interface to the PFsense firewall to account for a real world scenario in which an attacker from another network can attack us. The IP address of the network is 72.146.234.1/24, this is the range of IPs that the attacker (kali linux machine) will have. The goal for adding the firewall rule is to block this attacker from being able to reach the enterprise in any manner.

The first thing I had to do was set up the firewall rule for OPT1, this is the Kali attackers network. 

![Screenshot 2025-04-29 190702](https://github.com/user-attachments/assets/682fa352-c61a-4de8-af20-763bb64a6c32)

This allows all traffic from OPT1. Why? I wanted to simulate unrestricted internet access, like real-world traffic that can ping, scan, or probe my internal network.

With this in place, the attacker can reach the firewall and even internal devices — which is a big deal if left unchecked.

## Step 3: Installing Snort

With potential attacks now possible, I needed a way to detect them. That’s where Snort comes in.

To install Snort:

Go to System → Package Manager

Search for and install Snort

Once installed, I navigated to Services → Snort and added interfaces for monitoring. I started with the LAN interface, since it represents the internal network I want to protect.

I set up a LAN rule to test to see if the alerts work down below:

![Screenshot 2025-04-30 185230](https://github.com/user-attachments/assets/e80c55c5-53ac-4005-a421-860e3870690d)

This rule just checks for any ICMP packets flowing out of the network. As we can see in alerts it actually worked!

![Screenshot 2025-04-30 185349](https://github.com/user-attachments/assets/17e537e6-5ab7-44d4-acb3-9f262b46fbe0)

Now the question is... how can I create a rule thats actually useful? 

To detect more advanced scanning behavior, I added the OPT1 interface to Snort — since this simulates the public-facing internet side of the firewall.

I then created a custom rule to detect Nmap SYN scans:

![image](https://github.com/user-attachments/assets/aed1ad7c-b224-4f45-bcbb-016abb4dd547)

To test this rule, I ran `nmap -sS 10.0.2.1` and went back to the alerts tab in PFsense to find a bunch of alerts had been fired off!

![Screenshot 2025-04-30 195503](https://github.com/user-attachments/assets/d9781d72-e7bf-4906-97aa-eb99faec75d2)

In a real-world scenario, after detecting malicious activity like this, I’d take immediate action — like blocking the attacker’s IP via firewall rules or escalating the alert to the SOC to see if they can find out more through their SIEM.

## Step 4: Less Talking, More Blocking

From the alert I got in Snort, I can immediately take action by blocking this malicious IP in the firewall.

I went to Firewall → Rules → OPT1 and added this rule below:

![Screenshot 2025-04-30 203623](https://github.com/user-attachments/assets/28830953-a412-4fc1-b565-ca875516101f)

What this rule does is block any packets from the IP address provided in the picture. 

When I tried to run an Nmap scan of 10.0.2.1 again, all of the packets dropped and the potential attack has been thwarted! (For now)

![image](https://github.com/user-attachments/assets/b69d62e7-47e4-4c0c-a327-4d473bc98866)

