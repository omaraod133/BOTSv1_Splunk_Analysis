# BOTSv1_Splunk_Analysis

## Objective

The Detection Lab project aimed to establish a controlled environment for simulating and detecting cyber attacks. The primary focus was to ingest and analyze logs within a Security Information and Event Management (SIEM) system, generating test telemetry to mimic real-world attack scenarios. This hands-on experience was designed to deepen understanding of network security, attack patterns, and defensive strategies.

### Skills Learned
- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
- Security Information and Event Management (SIEM) system for log ingestion and analysis.

## overview 
Boss of the SOC (BOTS) is a Blue Team training exercise and competition developed by Splunk. It is designed to simulate real-world security incidents using large and realistic datasets.
In BOTSv1, participants are given logs from different sources such as **Windows event logs**, **firewall logs**, and intrusion detection system **(IDS) logs**. These datasets contain both normal activity and malicious behavior.
**The goal** is to investigate the data and answer a series of questions based on your findings. This helps analysts practice detecting threats, understanding attack patterns, and improving their incident investigation skills.

### Q1:What is the likely IPv4 address of someone from the Po1s0n1vy group scanning imreallynotbatman.com for web application vulnerabilities?


1. We know from the question that this group is scanning for web vulnerabilities, meaning they send a large number of malicious payloads to test the site.
    
2. So, we will check our firewall logs to see which IP is sending a large number of requests.
    
<img width="1845" height="664" alt="{DBD1EEFB-FEAD-4B94-9D82-1AD772022B96}" src="https://github.com/user-attachments/assets/8837a5d4-bf1c-4909-9611-e39b29f28ab7" />
Here, we can see that there are two IP addresses sending a large number of requests to the website:

- 40.80.148.42
    
- 23.22.63.114
    

You can test either of these, but we will dive a bit deeper to determine which one performed the scan.

We will examine the firewall logs for the IP address **40.80.148.42** and look at the signature field. We can see that there is scanning activity identified as **Acunetix Web Vulnerability Scanner**.

<img width="1849" height="906" alt="{BEF9A2DB-46EB-4106-BC55-285372E5A80A}" src="https://github.com/user-attachments/assets/df2636d8-3929-4a6a-928a-e8d76509514f" />

The scan started at **2016-08-11 00:36:45** and ended at **2016-08-11 00:54:30**, lasting about **18 minutes**.

<img width="1915" height="918" alt="{3F7D6357-8F8F-498B-B01C-48711FCD989C}" src="https://github.com/user-attachments/assets/d8fc9f57-8b29-421a-959e-e07a1b87a4a1" />
So, the answer to the first question is: **40.80.148.42**

### Q2:



