# TShark Challenge I: Teamwork - Write-Up

## Overview

This write-up documents the investigation of suspicious domain traffic using TShark and VirusTotal analysis as part of the SOC team's incident response.

---

## Task 1: Introduction

### Objective
Investigate captured traffic data and analyze suspicious domain activity to create detection artifacts.

### Tools Used
- **TShark** - Command-line packet analyzer for network traffic inspection
- **VirusTotal** - Malware and threat intelligence detection platform
- **CyberChef** - Web-based tool developed by for performing various cyber operations like encoding, encryption, and data analysis

### Setup
- VM: Started successfully via split view
- Exercise files: `~/Desktop/exercise-files`
- Prerequisites: Completed TShark: The Basics and TShark: CLI Wireshark Features

**Note:** Exercise files contain real examples. Do NOT interact with them outside the given VM.

<h2>Deliverables</h2>

**Read the task above and start the attached VM.**

> **Answer**: No answer needed
---

## Task 2: Case: Teamwork!

An alert has been triggered: “The threat research team discovered a suspicious domain that could be a potential threat to the organisation.”

The case was assigned to you. Inspect the provided teamwork.pcap located in ~/Desktop/exercise-files and create artefacts for detection tooling.

Your tools: TShark, VirusTotal.

<h2>Deliverables</h2>
Answer the questions below

Investigate the contacted domains.
Investigate the domains by using VirusTotal.
According to VirusTotal, there is a domain marked as malicious/suspicious.

<h3>What is the full URL of the malicious/suspicious domain address? Enter your answer in defanged format.</h3>

I first started my investigation by reading the "teamwork.pcap" file to get a baseline of the URLs displayed, then pulled the DNS queries from it using the appropriate tshark search filters, piping it to the "nl" command, which numbers the lines of a file.

### Command Executed
```bash
tshark -r teamwork.pcap -Y "dns.qry.type == 1 " | nl
```

After scanning through the packets and filtering DNS queries for all A queries, I stumbled upon a suscpiciously looking domain:

### Command Output

<div align="center">
  
**Figure 1:** Output of TShark command showing extracted domains

![DNS Queries](https://github.com/amoham001/Write-Ups/blob/5288f90237bca243e4fd28b1750423fcdfdb362c/TryHackMe/CTFs/TShark%20Challenge%20I%20-%20Teamwork/Screen%20Captures/DNS-Qry-Output.png)

</div>

The supsicously looking domain can be seen above due to its unusual characters.

Now, we can defang the URL using <a href=https://cyberchef.org/>CyberChef</a>



> **Answer**: hxxp[://]www[.]paypal[.]com4uswebappsresetaccountrecovery[.]timeseaways[.]com/

<h3>When was the URL of the malicious/suspicious domain address first submitted to VirusTotal?</h3>

I pasted the URL above into VirusTotal and got this result.

<div align="center">
  
**Figure 2:** VirusTotal Domain First Submission

![Virus Total First Submission](https://github.com/amoham001/Write-Ups/blob/5288f90237bca243e4fd28b1750423fcdfdb362c/TryHackMe/CTFs/TShark%20Challenge%20I%20-%20Teamwork/Screen%20Captures/VirusTotal_First_Submission.png)

</div>

As can be seen in the History section within the Details tab, the URL was first submitted to VirusTotal on April 17th, 2017 @ 22:52:53 UTC

 
> **Answer:** 2017-04-17 22:52:53 UTC

<h3>Which known service was the domain trying to impersonate?</h3>

This is pretty straightforward as Paypal is what the URL begins with.


> **Answer:** Paypal

<h3>What is the IP address of the malicious domain? Enter your answer in defanged format.</h3>

Using VirusTotal again, I scanned the URl, only this time without the "http://", and found the malicious domain IP address

<div align="center">

** Figure 3:** VirusTotal Query No "http://"

![VirusTotal No HTTP](https://github.com/amoham001/Write-Ups/blob/5288f90237bca243e4fd28b1750423fcdfdb362c/TryHackMe/CTFs/TShark%20Challenge%20I%20-%20Teamwork/Screen%20Captures/VTotal-Nohttp.png)
</div>

<div align="center">

**Figure 4:** Malicious Domain IP Address

![Malicious Domain IP Address](https://github.com/amoham001/Write-Ups/blob/5288f90237bca243e4fd28b1750423fcdfdb362c/TryHackMe/CTFs/TShark%20Challenge%20I%20-%20Teamwork/Screen%20Captures/VTotal_IP.png)

</div>

After defanging with CyberChef, this is the answer

> **Answer:** 184[.]154[.]127[.]226

<h3>What is the email address that was used? Enter your answer in defanged format. (format: aaa[at]bbb[.]ccc)</h3>

Given that the victim's credentials were most likely uploaded to this fake site, I used the following tshark command to filter for HTTP POST requests, using the -V flag to return all packet information

### Command Executed
```bash
tshark -r teamwork.pcap -Y 'http.request.method == POST' -V
```

### Command Output
After executing this command, the output shows the compromised account credentials, which includes our wanted email address.

<div align="center">

**Figure 4:**: Used Email Address

![Used Email Address](https://github.com/amoham001/Write-Ups/blob/5288f90237bca243e4fd28b1750423fcdfdb362c/TryHackMe/CTFs/TShark%20Challenge%20I%20-%20Teamwork/Screen%20Captures/Email_Location_Output.png)

</div>


> **Answer:** johnny5alive[at]gmail[.]com

Congratulations! You have finished the first challenge room, but there is one more ticket before calling it out a day!

    TShark Challenge II: Directory

> **Answer:** No answer needed

---
<h3>End Note</h3>

After completing this challenge I have gained valuable experience using tshark and realized the efficiency of the command line for packet analysis. Congratulations to all those who completed this challenge, I hope you found my walkthrough helpful!

You can find more of my walkthroughs <a href=https://github.com/amoham001/Write-Ups/tree/main/TryHackMe>here</a>





