<img width="905" height="383" alt="WhatsApp Image 2026-04-23 at 3 53 40 PM (2)" src="https://github.com/user-attachments/assets/6c68abb2-1afb-44f2-a665-da98e047f4e1" /># TryHAckME-LianYU
WEEK 6
Room Link: https://tryhackme.com/room/lianyu

## 🎯 Main Objective
👉 Compromise the target machine and gain root access

That means:

Break into the system
Escalate privileges
Capture flags (proof of access)

## Step 1: Scanning the IP
```
nmap 10.49.165.124
```
Findings:

Port 21 = FTP
Port 22 = SSH
Port 80 = HTTP
Port 111 = rpcbind
<img width="905" height="383" alt="WhatsApp Image 2026-04-23 at 3 53 40 PM (2)" src="https://github.com/user-attachments/assets/5ab15e73-89da-4444-9d9f-223879e39ded" />

## Step 2: Enumeration
Visit the target IP in browser
```
http://10.49.165.124
```
<img width="901" height="769" alt="WhatsApp Image 2026-04-23 at 3 53 40 PM (4)" src="https://github.com/user-attachments/assets/c2baf49a-5bed-47a2-b155-3a758930f12d" />

Then, run gobuster for hidden directories.
```
gobuster dir -u http://10.49.165.124 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

