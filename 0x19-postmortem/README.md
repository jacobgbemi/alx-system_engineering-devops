# 0x19-postmortem

![Postmortem](https://github.com/jacobgbemi/alx-system_engineering-devops/blob/main/0x19-postmortem/postmortem.png)
## Service Failure - Incident Report on Site Outage

### Issue Summary
On December 4th, 2022 from 6:30 AM to 7:00 AM WAT, the company's website was down for thirty minutes. 100% of users experienced a 500 internal server error caused by a wrong filename in a configuration file in the NGINX server.

### Timeline
6:30 AM Outage begins
6:40 AM Error logs checked by DevOps team
6:43 AM Configuration updated to log extra errors
6:45 AM Filename error caught in config file
6:47 AM Execute Puppet manifest across all company servers
7:49 AM 100% restored and back online

### Root Cause and Resolution
After a small sitewide update was deployed without first being tested, outage to the site began. When no errors were found in our 'error.log' files we modified our configuration file to allow for more erorr logging. With the help of 'strace,' it appeared an accidental filename mispelling triggered errors when the site was requested. The server was attempting to locate the file as normal procedure but failed to find the file ending in ".phpp" when it should be looking for ".php" After changing this line in the '/var/www/html/wp-settings.php' file, tests suceeded to show the site. A puppet manifest was written and executed across all company servers immediately after to restore service.

### Corrective and Preventative Measures
After a discussion it has been decided we can prevent this in the future by:

Modifying configuration files for more error logging to quicken response times
Test before deploying on all servers no matter how small an update. 
