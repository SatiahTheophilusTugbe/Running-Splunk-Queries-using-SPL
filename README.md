# RUNNING SPLUNK QUERIES USING SPL
# Project Overview

In this project, I document my journey in exploring various aspects of Splunk. In this guide, I'll walk you through the steps I took to complete key activities related to Splunk, 
from initial data injestion warm-ups to scheduling alerts. This repo serves as a detailed reference for anyone looking to understand how to perform these tasks in Splunk.

# Data/Log Injestion Warm ups
In this initial exercise, I designed a custom SPL (Search Processing Language) query and researched specific security mitigation strategies for an attack. Here’s how I approached it:

- Uploading the Log File: I started by uploading the provided Fortinet IPS log file into Splunk.

![3](https://github.com/user-attachments/assets/04364515-2601-4bc6-a51b-6fb62d9f5d5e)

- Analyzing Source IP: Using the query, I determined the top source IP (src_ip) which turned out to be 41.146.8.66 with 22 counts.

![8](https://github.com/user-attachments/assets/9ad43eb0-d8f0-4fdc-9d85-5932985ab5ed)

- Identifying the Attack: The logs revealed that the primary attack was labeled Oracle.9i.TNS.OneByte.DoS.

![11](https://github.com/user-attachments/assets/eef3b8f4-7278-4319-a2f7-772a2692b0ba)

# Mitigation Strategy: 
The attack was a Denial of Service (DoS) targeting a TNS listener. I found that patching or upgrading to a non-vulnerable version is a recommended mitigation.

# Outcome: 
This activity gave me a strong understanding of how to utilize Splunk for forensic analysis and security threat identification.

# Splunk Statistics
Next, I was tasked with creating statistical reports and generating a new field using SPL:

- Top Destination IPs: I used a query to list the top 20 destination IP addresses.

SPL Query: source="fortinet_IPS_logs.csv" | top limit=20 dest_ip

![14](https://github.com/user-attachments/assets/f745df37-bc88-4b3b-acc5-2bad8a8a196b)

- Top Source IPs and Ports: I created another query to display the top 10 source IPs and their associated ports.

SPL Query: source="fortinet_IPS_logs.csv" | top limit=10 src_ip src_port

![16](https://github.com/user-attachments/assets/4bfb66eb-947f-48c2-be67-edb6ff4d942c)

- Custom Field Creation: I created a new field DB_type to distinguish between customer and non-customer databases.

SPL Query: source="fortinet_IPS_logs.csv" | eval DB_type = if(dest_ip == "12.130.60.5","Customer DB","Non-Customer DB")

![17](https://github.com/user-attachments/assets/f49099d0-d3eb-4f32-8b33-34b35f40438f)

# Outcome: 
This activity honed my ability to use SPL for generating meaningful reports and creating custom fields for better data classification.

# Splunk Reports
In this task, I created a scheduled report and set up an email notification system:

- Creating a Report: I generated a report titled "BruteForce by account_Name Report" using the query from the previous activity.

![19](https://github.com/user-attachments/assets/65e99d3a-5d38-476e-afa3-d587860f98a5)

- Scheduling the Report: I scheduled the report to run daily at 01 a.m. and set up an email notification to be sent to management.

![20a](https://github.com/user-attachments/assets/9036a46e-9eb6-4c60-919b-c0f09e431392)

- Email Notification setup:

![20b](https://github.com/user-attachments/assets/b1a87dbc-cc3c-4072-880e-325fc21972ff)

- Report creation confirmation:

![21](https://github.com/user-attachments/assets/52c20e14-3591-4109-b28c-36ddd201fd18)

# Outcome: 
This exercise solidified my skills in automating report generation and setting up proactive notification systems in Splunk.

# Splunk Baselining
Here, I established a baseline for normal activity on a Windows server:

<img width="928" alt="baseline1" src="https://github.com/user-attachments/assets/a46a9b2a-fffe-4f8f-a9c5-625af71141c3">

- Uploading Windows Logs: I started by uploading the provided windowsrawlogs.txt file.

![image](https://github.com/user-attachments/assets/6b505087-e668-4dcf-a4be-c364a4635be4)

- Analyzing Failed Logins: I analyzed failed login attempts (EventCode=4625) to identify unusual activity, noticing a spike at 7 a.m. on February 11, 2020.

![image](https://github.com/user-attachments/assets/3fcb9a1b-a37b-4e4d-93f8-01ac75d2ba7b)

- EventCode 4625

![image](https://github.com/user-attachments/assets/1ef413cb-5513-41bc-998c-1faeb713f8c8)

Setting Thresholds: Based on the average failed logins, I set a threshold of 7 logins per hour as a baseline for detecting brute force attacks.

# Outcome: 
This task enhanced my understanding of baselining and how to set effective thresholds for alerting on suspicious activity.

# Splunk Scheduling Alerts
In the final task, I designed and scheduled an alert to notify when a brute force attack is detected:

Alert Configuration: I set up an alert to trigger if the number of failed logins exceeds the threshold of 7 within an hour.
Email Notifications: I configured the alert to send an email to the SOC team with a subject line "Brute Force Alert".
Outcome: This activity rounded out my learning by allowing me to set up real-time alerts, which is critical for timely incident response.

Feel free to explore the individual files and queries in this repository to get a more detailed understanding of each step. This repository documents my hands-on experience and serves as a guide for others venturing into the world of Splunk.

