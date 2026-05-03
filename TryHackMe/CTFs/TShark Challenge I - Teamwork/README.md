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

- **Answer**: No answer needed
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

#### Command Executed
```bash
tshark -r teamwork.pcap -Y "dns.qry.type == 1 " -T | nl
```

### Command Output

![DNS Queries](<img width="2852" height="854" alt="DNS-Qry-Output" src="https://github.com/user-attachments/assets/15e2078b-9a27-4ac6-96c9-b0ca82a283fb" />)
