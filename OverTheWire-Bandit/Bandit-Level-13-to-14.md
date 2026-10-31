# OverTheWire: Bandit — Level 13 to Level 14 Write-up

📖 **Read the fully formatted article with code highlights on my [Medium]([https://medium.com](https://medium.com/@ismayil.2006jj/overthewire-bandit-level-13-14-walkthrough-updated-2026-c22efc4d2ee7)) page.**

---
### 📝 Challenge Description
OverTheWire: Bandit Level 13 → 14 Walkthrough (Updated 2026)

🎯 The Mission
Instead of a standard password, this level gives us a private SSH key (sshkey.private) to log into bandit14. Our ultimate goal is to connect successfully and read the next flag stored at /etc/bandit_pass/bandit14.

Introduction
<img width="828" height="296" alt="image" src="https://github.com/user-attachments/assets/37dfad97-3bb4-4a59-a803-343fb6076af5" />
The Concept: SSH Private Keys as Digital Badges

In this level, we are not given a standard password text file for the next user. Instead, we find a file named sshkey.private.

🚨 The Problem: Broken Walkthroughs
Standard walkthroughs instruct you to run the following command directly inside the server:

ssh -i sshkey.private bandit14@localhost -p 2220
If you try this today, the server will immediately terminate your session with an error like Connecting from localhost is blocked or Connection refused. The platform administrators intentionally blocked internal loopback SSH connections to conserve server resources.

<img width="955" height="512" alt="image" src="https://github.com/user-attachments/assets/963bc0e2-1190-49fe-a2f1-ff289d690914" />

The Solution: Data Exfiltration & Remote Authentication
The strategy here is to exfiltrate (download) the private key to our local machine (Kali Linux) and authenticate directly from the outside.

Step 1: Downloading the Key via SCP
We log out of the server, open our local Kali Linux terminal simply , and use (SCP) command to pull the file to our linux machine, like this;

scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private .
(Note: You will be prompted to type the password for bandit13 to authorize the download. Don't forget the trailing dot . which represents your current local directory).


<img width="1400" height="507" alt="image" src="https://github.com/user-attachments/assets/2c904b0a-8c43-42e3-b7bf-e6a0c731cbb8" />
downloaded succesfully

Step 2: Fixing File Permissions (chmod)
SSH clients have strict built-in security mechanisms. If a private key file is accessible by other users on the system, the SSH client will reject it as insecure. We must restrict the permissions so only our user can read it:
<img width="1400" height="537" alt="image" src="https://github.com/user-attachments/assets/d7d0366c-4526-4169-9574-a42c87ee7a2c" />
chmod 600 sshkey.private

Step 3: Direct Remote Infiltration
With the key securely hosted on our local machine and the correct permissions set, we completely bypass the restricted localhost environment. We connect straight to bandit14 from the outside:

ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
<img width="1132" height="422" alt="image" src="https://github.com/user-attachments/assets/a4d00c06-c6c2-43ec-ba68-8ff0349ee0ce" />

“We have successfully gained access and established an initial foothold as bandit14.

Step 4: Retrieving the Flag
Now that we are officially authenticated as bandit14, we can access the protected directory to retrieve our official password for the next stage:

cat /etc/bandit_pass/bandit14

<img width="896" height="129" alt="image" src="https://github.com/user-attachments/assets/e93af439-88f8-4f2b-8152-f7c9cbdf643c" />













