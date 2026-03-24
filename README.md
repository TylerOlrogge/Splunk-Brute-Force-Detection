Splunk-Brute-Force-Detection Lab

#Overview: 
Setting up Splunk Enterprise to detect failed logon attempts. 

I created an alert within Splunk that triggers whenever there are at least 3 failed logon attempts (potential failed brute-force attack) as well as an alert that triggers whenever there are at least 3 failed logon attempts followed by a successful logon (potential successful brute-force attack).

#Setup: 
- Windows Server 2025 funning on AWS EC2
- Splunk Enterprise

#Key Concept:

#Detection Rule: 

#Alert Setup:

#Testing: 

#What I Learned

#Problems I ran into

#Summary
