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

<img width="942" height="536" alt="WhatsApp Image 2026-04-23 at 3 53 41 PM (1)" src="https://github.com/user-attachments/assets/ab822633-90d4-4969-8b26-e0e4acd428d8" />

Now doing the same again go to the browser and search
http://10.49.165.124/island/2100

<img width="954" height="854" alt="WhatsApp Image 2026-04-23 at 3 53 41 PM (2)" src="https://github.com/user-attachments/assets/0a3037dd-aa33-4fc2-b31f-f9b866d52618" />

View the page source again cause usually they like to hide something here--

<img width="938" height="861" alt="WhatsApp Image 2026-04-23 at 3 53 41 PM (3)" src="https://github.com/user-attachments/assets/5b47e662-ef1d-4b49-8438-e98f79ebae72" />

Here it says there is a file with a '.ticket' extension. Run gobuster to find .ticket as the hint said

```
gobuster dir -u http://10.49.165.124/island/2100/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x ticket
```
<img width="932" height="543" alt="WhatsApp Image 2026-04-23 at 3 53 41 PM (4)" src="https://github.com/user-attachments/assets/3c95f8b2-8411-4b6c-9380-c5b58f915e47" />

Found another director : /green_arrow.ticket

Again going to the browser search
http://10.49.165.124/island/2100/green_arrow.ticket.

<img width="943" height="866" alt="WhatsApp Image 2026-04-23 at 3 53 41 PM (5)" src="https://github.com/user-attachments/assets/529b0d47-9bd1-4202-bc28-72e62be3f8c1" />

Found an encryption : 'RTy8yhBQdscX' . So now lets try to decode it ---

Go to https://gchq.github.io/CyberChef/

Use 'FromBase58' to decode it.

<img width="1600" height="766" alt="WhatsApp Image 2026-04-23 at 3 53 41 PM (6)" src="https://github.com/user-attachments/assets/88e631d5-172e-4812-b8d4-5f4e31210158" />
We have cracked it : '!#th3h00d' - This is the FTP Password.

## Step 3: FTP Login
Now we have the username and password Username - vigilante Password - 
!#th3h00d

<img width="935" height="236" alt="WhatsApp Image 2026-04-23 at 3 53 41 PM (7)" src="https://github.com/user-attachments/assets/a39652ba-ff89-4a1e-abc5-8ed514b82143" />
Once success to login ftp, use 'ls' command to look for image files in the server. If find any files there, download all the files using this command

```
mget *
```
<img width="907" height="592" alt="WhatsApp Image 2026-04-23 at 3 53 41 PM (8)" src="https://github.com/user-attachments/assets/31bf18fd-294f-4bca-9311-0b4ade55db9f" />

<img width="538" height="370" alt="Screenshot 2026-04-27 120333" src="https://github.com/user-attachments/assets/0f8790c9-d036-4c1f-862b-ffbc598c5d5d" />

<img width="393" height="624" alt="Screenshot 2026-04-27 120356" src="https://github.com/user-attachments/assets/2a115d8d-e908-475d-8b9b-c6439e508eb7" />

<img width="478" height="430" alt="Screenshot 2026-04-27 120419" src="https://github.com/user-attachments/assets/e478a4c4-9285-43e6-87aa-3bc9ac005be2" />
I checked the image use hex editor. The first 8 bit of the Leave_me_alone.png is not correct according to png format.

<img width="536" height="394" alt="Screenshot 2026-04-27 120448" src="https://github.com/user-attachments/assets/c1e6ed75-36a4-464b-aa90-378b69ac2640" />

<img width="537" height="379" alt="Screenshot 2026-04-27 120506" src="https://github.com/user-attachments/assets/4209923e-94bf-4b04-a576-73682d44dd62" />
After open the photo, the password is given for steghide. Try to enter the password when use steghide command on the png file.

```
steghide extract -sf aa.jpg
```
<img width="932" height="800" alt="WhatsApp Image 2026-04-23 at 3 53 45 PM" src="https://github.com/user-attachments/assets/49f1530f-fe08-4d24-b74b-71ad197bac08" />
Now using the password 'password' we got earlier successfully extracted the .jpg file to a ss.zip file. We found a a 'passwd.txt' and a 'shado file' unzipping the ss.zip file.

Now cat 'shado' file and you get a password : 'M3tahuman' -- (ssh password)

Now as we have got the ssh password we can now login -- User - slade password - 
M3tahuman

```
ssh slade@10.49.165.124
```
<img width="970" height="869" alt="WhatsApp Image 2026-04-23 at 3 53 45 PM (1)" src="https://github.com/user-attachments/assets/6ca69efe-72c7-4bdb-9ad9-485f14c2f080" />
Once enter ssh, read the file user.txt

<img width="947" height="865" alt="WhatsApp Image 2026-04-23 at 3 53 40 PM" src="https://github.com/user-attachments/assets/68fd1855-69ec-4bd8-9534-3612ee1d4efe" />

Then run
```
sudo -l
```

then run, to access root

```
sudo pkexec su
```

Once run the command, type 'ls' and it will show root.txt file. Read the file using cat command

```
cat root.txt
```

And mission accomplished.

<img width="943" height="877" alt="WhatsApp Image 2026-04-23 at 3 53 40 PM (1)" src="https://github.com/user-attachments/assets/aa73976c-429c-4e79-b725-d8a4bb6b747e" />









