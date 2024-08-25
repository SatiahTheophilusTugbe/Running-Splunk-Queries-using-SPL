# RUNNING SPLUNK QUERIES USING SPL
# Project Overview

In this project, I document my journey in exploring various aspects of Splunk. In this guide, I'll walk you through the steps I took to complete key activities related to Splunk, 
from initial data injestion warm-ups to scheduling alerts. This repo serves as a detailed reference for anyone looking to understand how to perform these tasks in Splunk.

# Data/Log Injestion Warm ups
In this initial exercise, I designed a custom SPL (Search Processing Language) query and researched specific security mitigation strategies for an attack. Here’s how I approached it:

<h2> Uploading the Log File:</h2>
  I started by uploading the provided Fortinet IPS log file into Splunk.

![3](https://github.com/user-attachments/assets/04364515-2601-4bc6-a51b-6fb62d9f5d5e)

- Analyzing Source IP:
  Using the query, I determined the top source IP (src_ip) which turned out to be 41.146.8.66 with 22 counts.

![8](https://github.com/user-attachments/assets/9ad43eb0-d8f0-4fdc-9d85-5932985ab5ed)

- Identifying the Attack: The logs revealed that the primary attack was labeled Oracle.9i.TNS.OneByte.DoS.

![11](https://github.com/user-attachments/assets/eef3b8f4-7278-4319-a2f7-772a2692b0ba)

# Mitigation Strategy: 
The attack was a Denial of Service (DoS) targeting a TNS listener. I found that patching or upgrading to a non-vulnerable version is a recommended mitigation.

# Outcome: 
This activity gave me a strong understanding of how to utilize Splunk for forensic analysis and security threat identification.

2. Splunk Statistics
Next, I was tasked with creating statistical reports and generating a new field using SPL:

Top Destination IPs: I used a query to list the top 20 destination IP addresses.
spl
Copy code
source="fortinet_IPS_logs.csv" | top limit=20 dest_ip
Top Source IPs and Ports: I created another query to display the top 10 source IPs and their associated ports.
spl
Copy code
source="fortinet_IPS_logs.csv" | top limit=10 src_ip src_port
Custom Field Creation: I created a new field DB_type to distinguish between customer and non-customer databases.
spl
Copy code
source="fortinet_IPS_logs.csv" | eval DB_type = if(dest_ip == "12.130.60.5","Customer DB","Non-Customer DB")
Outcome: This activity honed my ability to use SPL for generating meaningful reports and creating custom fields for better data classification.

3. Splunk Reports
In this task, I created a scheduled report and set up an email notification system:

Creating a Report: I generated a report titled "DB Server Attack Report" using the query from the previous activity.
Scheduling the Report: I scheduled the report to run daily at 12 p.m. and set up an email notification to be sent to management.
Outcome: This exercise solidified my skills in automating report generation and setting up proactive notification systems in Splunk.

4. Splunk Baselining
Here, I established a baseline for normal activity on a Windows server:

Uploading Windows Logs: I started by uploading the provided windowsrawlogs.txt file.
Analyzing Failed Logins: I analyzed failed login attempts (EventCode=4625) to identify unusual activity, noticing a spike at 7 a.m. on February 11, 2020.
Setting Thresholds: Based on the average failed logins, I set a threshold of 7 logins per hour as a baseline for detecting brute force attacks.
Outcome: This task enhanced my understanding of baselining and how to set effective thresholds for alerting on suspicious activity.

5. Splunk Scheduling Alerts
In the final task, I designed and scheduled an alert to notify when a brute force attack is detected:

Alert Configuration: I set up an alert to trigger if the number of failed logins exceeds the threshold of 7 within an hour.
Email Notifications: I configured the alert to send an email to the SOC team with a subject line "Brute Force Alert".
Outcome: This activity rounded out my learning by allowing me to set up real-time alerts, which is critical for timely incident response.

Feel free to explore the individual files and queries in this repository to get a more detailed understanding of each step. This repository documents my hands-on experience and serves as a guide for others venturing into the world of Splunk.

