# Incident Response Lab

## Introduction
In this project, I used the Computer Security Incident Handling Guide(NIST 800-61) in my SOC/Honeynet environment to solve an incident, which involved a successful brute force attempt into my Windows Virtual Machine in Microsoft Azure. The environment used in this project is in relation to my other project: [Creating a Live SOC/Honeynet in Azure](https://github.com/James-Jeudy/SOC-Honeynet-Azure).

![NIST 800-61 Framework](https://github.com/James-Jeudy/Incident-Response/assets/160562010/63fb401a-8a87-4293-9c9d-d5cbedb5f2f7)



## Step 1: Preparation:
This was initiated already by ingesting all of the logs into Log Analytics Workspace and Microsoft Sentinel and configuring alert rules in my SOC/Honeynet project. 

We turned off the VM immediately after being notified of the incident to prevent any further malicious activity: <br>

<br>

<br>


![Immediately stop the VM](https://github.com/James-Jeudy/Incident-Response/assets/160562010/a7b25b6e-a034-4e93-ba05-5f2b4dc6fd3e) <br>

<br>

<br>

<br> 

## Step 2: Detection & Analysis

<h3>1. Set Severity, Status, & Owner<h3></h3>

I set the severity to High as it was a successful brute force attempt into my Windows VM. I set the status to active as I was currently working on the ticket and the owner as Myself:

<br>

![Set the owner status   severity for incident](https://github.com/James-Jeudy/Incident-Response/assets/160562010/d6a28555-52fc-4fff-a717-8170c46beeac)

<br> 

<h3>2. View Full Details<h3></h3>

![View full details of the incident](https://github.com/James-Jeudy/Incident-Response/assets/160562010/6b74c7a6-6165-4b94-af83-f459d2ff983d)


<br>


 <h3> 3. Observe the Activity Log (for history of incident):<h3></h3>

![Incident Activity Log](https://github.com/James-Jeudy/Incident-Response/assets/160562010/7b3eae0a-c9d2-4e13-922a-df3b47220eb9)

<br>

<h3> 4. Observe Entities and Incident Timelines<h3></h3>

We found it very strange that there were five alerts for the successful brute force attempts as you should only need one successful attempt to access the machine. The Windows-VM is the machine that was accessed by malicious actors, and 24.46.222.79 is the IP address of the attacker. According to this info below, the attacker is located in Brooklyn, New York:

<br>

<br>



![Incident Timeline](https://github.com/James-Jeudy/Incident-Response/assets/160562010/f143c3b1-6ec2-4bb1-ac8a-836b6b3e27a9)

<br>

![Entities for incident person place or thing](https://github.com/James-Jeudy/Incident-Response/assets/160562010/091d9f00-8d08-4084-9380-0a77aac0856c)

<br> 

<br>


![Location of Attacker- further details](https://github.com/James-Jeudy/Incident-Response/assets/160562010/1a24c491-d531-46f3-be28-147ebaeeabb2)

<br>

<br>

<h3>5. “Investigate” the incident and continue trying to determine the scope<h3></h3>

<br>

<br>

![Investigate the incident](https://github.com/James-Jeudy/Incident-Response/assets/160562010/07beacc7-296a-4537-aeeb-6017c27fabdc)

<br>

<br>


![Investigate incident screen](https://github.com/James-Jeudy/Incident-Response/assets/160562010/557d53b1-05c7-4e2c-a2b8-f9f99aadf284)

<h3>6. Inspect the entities and see if there are any related events<h3></h3>

The IP Address 24.46.222.79 is also involved in five other brute force attempts, 11 brute force successes, 35 privilege escalations, etc. Potentially comprimised system "Windows-VM" is also involved in several other incidents/alerts. We suspect there is possible overexposure to the Public Internet.

![Right click attacker IP to get related alerts to see if there is any other malicious activity](https://github.com/James-Jeudy/Incident-Response/assets/160562010/18ec758c-6a56-4362-9471-91c40c40e55d)

![Lots of other brute force success   attempts from this IP address](https://github.com/James-Jeudy/Incident-Response/assets/160562010/330021c1-139f-41b3-8a48-c6d9f17dc4d6)

![Check related alerts of windows VM](https://github.com/James-Jeudy/Incident-Response/assets/160562010/bb246031-2965-4e61-bec4-77e044bafd37)

![This machine is involved with numerous alerts for obvious reasons](https://github.com/James-Jeudy/Incident-Response/assets/160562010/a0401fdb-1198-478d-bdcd-b77d541cdb61)

<h3>7. Determine legitimacy of the incident (True Positive, False Positive, etc.)<h3></h3>



As seen below, we ran some queries to determine if this incident is a true positive. The first query gathers failed logins within a one hour time period and some successful logins as well, and whenever someone has failed five or more times but also had a successful login in the same time period, we assume that it was an successful  brute force attempt. 

The second query gathers successful logins (Event ID: 4624) with the Attack IP Address of: 24.46.222.79 from James-Jeudy Laptop. Alerts are accurate as it was raised by successful logins from this workstation using the lab-user username. This is the local username of our Windows-VM machine. We determined that this is a true positive and we are lucky as no damage occured with the malicious actor being logged into the machine as this is a honeynet environment designed to attract attackers:

![Query in log analytics workspace](https://github.com/James-Jeudy/Incident-Response/assets/160562010/a7bdd65a-e4cc-40ad-8a9f-9f3f3343602f)

![As noted there was custom brute force logins with account on machine](https://github.com/James-Jeudy/Incident-Response/assets/160562010/3d976e3e-5580-4c6e-a9b9-6a1498047a0c)

![Brute force success from laptop](https://github.com/James-Jeudy/Incident-Response/assets/160562010/88bae0d4-83ad-4c71-86a5-5a05d69fec4d)

## Step 3: Containment, Eradication, & Recovery:

Since we already turned off the machine as our first step in the incident to prevent any further malicious activity, the next step for containment is to reset the password of our labuser account which is our local user account on the Windows-VM machine. 

To fully remediate the issue, we also locked down our Network Security Group for our Windows VM to just my IP address. Since the rule is setup to only allow my IP address, malicious actors won't be allowed in. It won't match any of the other rules listed here except the Denyallinbound rule which means every IP address besides my own will be denied. The firewall on the Windows-VM was also enabled. 

        
![Reset password for lab-user](https://github.com/James-Jeudy/Incident-Response/assets/160562010/9ba7b46b-d4fd-402b-9849-698f9822bc3e)

<br>

<br>


![Rules for Windows VM-NSG's](https://github.com/James-Jeudy/Incident-Response/assets/160562010/6209f9f3-d70a-4de0-9238-51ffe9784821)

<br>

<br>

## Step 4: Document Findings/Info and Clouse out the Incident in Sentinel

After gathering up all of the findings, we closed the ticket as a benign positive. This was suspicious but expected obviously, as this is a Honeynet environment designed to attract malicious attackers.  

![Click apply and then close incident](https://github.com/James-Jeudy/Incident-Response/assets/160562010/433a7467-c4fa-4e18-86b5-84ee8455ee35)

<br>

## Step 5: Conclusion:

In conclusion, this attack was determined as a true positive as a malicious attacker had a successful brute force attempt into our Windows-VM. Due to this being a honeynet environment, no important data was lost as this environment is meant to attract attackers.



