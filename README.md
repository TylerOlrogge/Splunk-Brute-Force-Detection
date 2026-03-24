# Splunk-Brute-Force-Detection:
- Setting up Splunk Enterprise to detect successful brute-force login attempts on a Windows device

#Objective:
- Set up Splunk so that it will alert when it detects a successful brute-force attack by utilizing Window logs. 

#Environment:
- AWS EC2
- Splunk Enterprise (Version 9.4.9)
- Windows Server 2025

#Steps for set up:
1. Download Splunk Enterprise on your machine and log in via the web interface. As of the time of writing this, I recommend version 9.4.9 as it seems to work better with local machine log collection. We will be collecting logs from the same machine that Splunk Enterprise was installed on.
2. At the top right, click on the drop down labeled "Settings", then click on or search for "Data Inputs"
3. The first option you will see is called "Local event log collection." On the right side of that, click on "Edit"
4. In the "Logs" section, select "Application", "Security", and "System" logs by clicking on them, and then click on the arrow pointing right. They should then appear under the "Selected log(s)" section.
5. Click "Save"
6. Click on the "Splunk Enterprise" icon at the top left to return to the home page
7. On the left side, click on "Search & Reporting"
8. In the search bar, enter the following search query: 

EventCode=4625 OR EventCode=4624
| stats
     count(eval(EventCode=4625)) as FailedLogonAttempts,
     count(eval(EventCode=4624)) as SuccessfulLogonAttempts
     by Account_Name
| where FailedLogonAttempts >3 AND SuccessfulLogonAttempts >0

9. You should then observe the Statistics, at the bottom of the screen, populate with some data. You should see your successful login attempts, and any unsuccessful login attempts made within the specified time frame of your search
10. At the top right, click on "Save As" and then click on "Alert"
11. You may give it any Title and Description that you wish. For this demonstration, we will keep the "Permissions" as "Private". Change the "Alert type" to "Scheduled", then click on the drop down right underneath of that. Click on "Run on Cron Schedule". Change the "Time Range" to "Last 15 minutes" and set the "Cron Expression" field to "*/5 * * * *". Scroll down to "Trigger Actions" and click on "Add Actions" and then "Add to Triggered Alerts". Finally, click on "Save"
12. Congratulations! The set up for Splunk alerting has been complete. If everything has been set up correctly, Splunk should check to see if there was at least 3 failed login attempts (EventCode=4625) followed by a successful login (EventCode=4624) and generate an alert.
13. Note: If you only want this to generate alerts when 3 failed login attempts have been made, regardless of if there is a successful login or not, remove the "AND SuccessfulLogonAttempts >0" from the query

#Testing:
1. Testing is very simple, simply lock your device and make at least 3 failed login attempts, followed by a successful login. Please keep in mind your local account lock-out policy, and you may want to consider making changes to there before simulating a brute-force attempt in your environment. 
