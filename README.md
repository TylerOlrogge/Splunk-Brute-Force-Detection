Splunk-Brute-Force-Detection Lab

#Overview: 
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

#Problems I ran into

#Summary
