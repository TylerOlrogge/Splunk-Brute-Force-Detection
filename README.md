Splunk-Brute-Force-Detection Lab

##Overview: 
Setting up Splunk Enterprise to detect failed logon attempts. 

I created an alert within Splunk that triggers whenever there are at least 3 failed logon attempts (potential failed brute-force attack) as well as an alert that triggers whenever there are at least 3 failed logon attempts followed by a successful logon (potential successful brute-force attack).

#Setup: 
- Windows Server 2025 running on AWS EC2
- Splunk Enterprise

#Key Concept:
-Monitor Windows Event logs: EventCode 4625 and EventCode 4624

#Detection Rule: 
EventCode=4625 OR EventCode=4624
| stats
     count(eval(EventCode=4625)) as FailedLogonAttempts,
     count(eval(EventCode=4624)) as SuccessfulLogonAttempts
     by Account_Name
| where FailedLogonAttempts >3 AND SuccessfulLogonAttempts >0

This ruls was designed to trigger if there are at least 3 failed logins, followed by a successful login, which could indicate a potential successful brute-force attack. SuccessfulLogonAttempts >0 can be removed if we are looking to monitor for failed brute-force attacks. 

#Alert Setup:
- Scans logs every 5 minutes 
- Reads through the last 15 minutes of data
- Triggers an alert if results match alert query

#Testing: 
- Entered the wrong password several times while at the login screen
- Verified that the logs appear correctly in Splunk
- Verified that the alert was triggered correctly

#What I Learned
- How to ingest Windows Event logs into Splunk Enterprise
- How to identify failed and successful login attempts by using Event ID 4625 and 4624
- How to write basic Splunk rules and how to test them
- How scheduled alerts with within SIEM environments

#Problems I ran into
- Splunk returned 404 errors when I tried assigning logs for collection (Resolved by downgrading to Splunk Enterprise version 9.4.9)
- Windows event logs not being ingested at first (Resolved by downloading the Splunk Add-on for Windows)
- Splunk would not allow me to access it via the web console (Resolved by utilizing net start/stop splunkd to restart the service) 

#Summary
This lab demonstrates how SIEM tools can be utilized to detect suspicious login attempts by analyzing Windows Event Logs. By building detection rule alerts within Splunk Enterprise, I was successfully able to simulate and identify potential brute-force attacks within a controlled environment. 
