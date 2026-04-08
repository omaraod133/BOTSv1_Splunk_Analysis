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

### Q2: What company created the web vulnerability scanner used by Po1s0n1vy? Type the company name.
We can answer this question from the previous one.

<img width="1849" height="906" alt="{BEF9A2DB-46EB-4106-BC55-285372E5A80R}" src="https://github.com/user-attachments/assets/66d25afc-a1eb-4725-99fd-080949459ec3" />

So, the correct answer is **Acunetix**.

### Q3:What content management system is imreallynotbatman.com likely using?

Let’s find out by searching for “content management” and seeing if there are any results.

<img width="1847" height="914" alt="{BF2EC3E1-0BE9-4A18-B02E-3FC90FB92C88D}" src="https://github.com/user-attachments/assets/d107ad3c-8bf8-4c3e-b24e-7fd304efabe0" />

We found that **Joomla** is the content management system used by imreallynotbatman.com.

### Q4: What is the name of the file that defaced the imreallynotbatman.com website? Please submit only the name of the file with extension?
This question is interesting. At first, I thought that maybe the attackers uploaded a defaced file, so I started searching for that.

Then I realized they might have found a way to make the imreallynotbatman.com website download the defaced file (which is what they actually did).

First of all, since the imreallynotbatman.com downloaded the file, this means the source IP would be the imreallynotbatman.com IP.
<img width="1851" height="913" alt="{8C3768B7-C2E8-48D2-AF61-062DDC158FE7}L" src="https://github.com/user-attachments/assets/873017fb-5c9e-4bfe-93ad-485bbf1ec88b" />

We can see that there are many records (about 765). If we scroll down a little and check the destination IPs filed, we notice something interesting: there is a suspicious IP address — **23.22.63.114**.


<img width="1852" height="893" alt="{2BAD2C1F-7A1F-4FAD-AF6F-4A9B6B8C35B2}M" src="https://github.com/user-attachments/assets/3252619b-5254-4937-afe6-fc322c534c74" />


If we add this IP (23.22.63.114) to our search as the destination IP address, we find that there are only two events.

<img width="1852" height="911" alt="{76AEF881-8D2F-40E5-AB0A-7C70FE1A2375}" src="https://github.com/user-attachments/assets/93b0197f-6330-4b6a-8732-da269f814076" />

If we examine those events closely, we can see the defaced file being downloaded from: **prankglassinebracket.jumpingcrab.com:1337**

<img width="1920" height="1080" alt="Screenshot (12)" src="https://github.com/user-attachments/assets/82eb4728-14aa-4b0b-b173-60e81f69741a" />

And the correct answer is **poisonivy-is-coming-for-you-batman.jpeg**

### Q5:This attack used dynamic DNS to resolve to the malicious IP. What fully qualified domain name (FQDN) is associated with this attack?
Since the question asks for the FQDN, we will change the `sourcetype` from `stream:http` to `stream:dns` to search for DNS events.

From the question, we need to find the FQDN associated with the malicious IP. We have two malicious IP addresses:

- 40.80.148.42
- 23.22.63.114
- 
We will search and check if there are any DNS events associated with them.  
First, we will start with **40.80.148.42**.

<img width="1856" height="907" alt="{29C0EC06-EE53-4F31-9F1B-58835F0E1B1D}0" src="https://github.com/user-attachments/assets/6b0d455e-5556-416f-8aa5-0c2282182cc0" />
As we can see, there are 0 events.

Now, let’s check the other IP:

<img width="1920" height="911" alt="{D10462CA-5CCB-400C-8F26-9F1DBCC30414}DN" src="https://github.com/user-attachments/assets/7a97f5d1-e66a-409b-9944-fe3e5c53b1c9" />

Here, we can see that there is 1 event, and the FQDN is:  
**prankglassinebracket.jumpingcrab.com**

This makes sense because, as we saw earlier, this is the website that **mreallynotbatman.com** visited to download the malicious file.


### Q6:What IPv4 address has Po1s0n1vy tied to domains that are pre-staged to attack Wayne Enterprises?

