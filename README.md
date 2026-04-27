TryHAckME-LianYU
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
<img width="897" height="539" alt="WhatsApp Image 2026-04-23 at 3 53 40 PM (3)" src="https://github.com/user-attachments/assets/8ea7ca95-1eab-4b3d-bf0c-ccb208a7170e" />

Findings: Found a directory: /island

After found the hidden directory, go to browser and search http://10.49.165.124/island/

<img width="900" height="773" alt="WhatsApp Image 2026-04-23 at 3 53 40 PM (5)" src="https://github.com/user-attachments/assets/1fd02d0f-6a6b-4750-906a-2df36005da63" />

<img width="902" height="780" alt="WhatsApp Image 2026-04-23 at 3 53 40 PM (6)" src="https://github.com/user-attachments/assets/023f70c1-e127-4bc2-b496-041628944888" />

<img width="898" height="780" alt="WhatsApp Image 2026-04-23 at 3 53 41 PM" src="https://github.com/user-attachments/assets/b2fbaafc-d4a4-4e94-98a5-9b9e71f24a6e" />

Found out the Code Word by highlighting the page text or viewing the page source. Code Word - 'vigilante' - (this is our FTP username)

Again run gobuster on /island directory to discover a different directory.

```
gobuster dir -u http://10.49.165.124/island -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```
Here we found another directory : /2100

<img width="932" height="543" alt="WhatsApp Image 2026-04-23 at 3 53 41 PM (4)" src="https://github.com/user-attachments/assets/0d547723-4c07-46e1-9b4c-3e7472750f25" />

Now doing the same again go to the browser and search
http://10.49.165.124/island/2100

<img width="954" height="854" alt="WhatsApp Image 2026-04-23 at 3 53 41 PM (2)" src="https://github.com/user-attachments/assets/0a3037dd-aa33-4fc2-b31f-f9b866d52618" />

View the page source again cause usually they like to hide something here--

<img width="938" height="861" alt="WhatsApp Image 2026-04-23 at 3 53 41 PM (3)" src="https://github.com/user-attachments/assets/5b47e662-ef1d-4b49-8438-e98f79ebae72" />

Here it says there is a file with a '.ticket' extension. Run gobuster to find .ticket as the hint said

```
gobuster dir -u http://10.49.165.124/island/2100/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x ticket
```







