# Target: Fawn (Hack The Box - Starting Point)

## 1. Overview
- Target IP: 10.129.230.109
- OS: Linux
- Difficulty: Very Easy
- Focus: Network Reconnaissance & FTP Anonymous Access

---

## 2. Reconnaissance & Enumeration

### Port Scanning & Service Version Detection
We performed an Nmap scan to identify active services and their versions on the target's open ports:

nmap -sV 10.129.230.109

<img width="1475" height="365" alt="1  Nmap Scan" src="https://github.com/user-attachments/assets/98162084-0b9d-4bf9-8b1c-672a4ae39ad4" />

Main findings:
- Port 21/tcp: Open FTP service running vsftpd 3.0.3 on Unix OS.

### FTP Client Parameter Verification
Before connecting, we reviewed the options for the ftp command using the -? help flag:

ftp -?

<img width="1767" height="918" alt="2  FTP Help" src="https://github.com/user-attachments/assets/834ad3b8-5b91-4e87-8f5d-0cf3f58b9c61" />

### Key Finding for Exploitation:
Reviewing the available parameters, we identified the `-a` flag (`Use anonymous login`). This confirms we can attempt an anonymous login, which is the exact misconfiguration we will exploit to gain access to the FTP server without valid credentials.

---

## 3. Exploitation

### Connection & Anonymous Login
We connected to the remote FTP server to test for anonymous access:

ftp 10.129.230.109

<img width="742" height="281" alt="3  FTP Connection" src="https://github.com/user-attachments/assets/7fd184a7-eb74-492b-affc-8eb1c027aa58" />

We authenticated using the default anonymous account:
- Name: anonymous
- Password: (press Enter / leave blank)

The server responded with status code 230 Login successful, confirming anonymous session access:

<img width="873" height="488" alt="4  FTP Login Successful" src="https://github.com/user-attachments/assets/3d9f4790-cb80-4f94-aff6-ff4da458a9c7" />

### Directory Inspection & Flag Extraction
We listed the remote directory contents to locate available files:

ftp> ls

<img width="1494" height="264" alt="FTP Directory Listing" src="https://github.com/user-attachments/assets/97fa1400-0f99-4419-8600-3c97dd8b004e" />

We downloaded the flag.txt file to our local machine using the get command:

ftp> get flag.txt

<img width="2208" height="360" alt="6  FTP Download Flag" src="https://github.com/user-attachments/assets/ceed2538-cc03-471b-b21a-a3693f53dc64" />

We exited the interactive FTP shell (exit) and read the downloaded flag on our local terminal:

cat flag.txt

<img width="799" height="164" alt="7  cat flag" src="https://github.com/user-attachments/assets/efc0aa5e-fc0b-48cb-9548-68bd5a13eb4a" />

Flag obtained: 035db21c881520061c53e0536e44f815

---

## 4. Key Takeaways & Commands Learned
- Nmap Version Detection (-sV): Useful for identifying exact service versions to assess potential vulnerabilities.
- FTP Anonymous Access: A misconfiguration where the FTP server allows authentication via the anonymous user without requiring a password, exposing sensitive files.
- Essential FTP commands:
  - ls: Lists files in the remote server directory.
  - get <file>: Transfers a file from the remote server to the local machine.
  - exit: Closes the active FTP session.
