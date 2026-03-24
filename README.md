Splunk-Brute-Force-Detection Lab

#Overview: 
Setting up Splunk Enterprise to detect failed logon attempts. 

I created an alert within Splunk that triggers whenever there are at least 3 failed logon attempts (potential failed brute-force attack) as well as an alert that triggers whenever there are at least 3 failed logon attempts followed by a successful logon (potential successful brute-force attack).

#Setup: 
- Windows Server 2025 funning on AWS EC2
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

This ruls was designed to trigger if there are at least 3 failed logins, followed by a successful login, which could indicate a potential successful brute-force attack. I would remove the SuccessfulLogonAttempts >0 if I wanted this to only count the failed login attempts, not the successful one as well. 

#Alert Setup:

#Testing: 

#What I Learned

#Problems I ran into

#Summary
