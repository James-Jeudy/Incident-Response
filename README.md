# Incident Response

## Introduction
In this project, I used the Computer Security Incident Handling Guide(NIST 800-61) in my SOC/Honeynet environment to solve incidents. This incident involves a simulated successful brute force attempt into my Windows-VM and goes into detail on how the incident was resolved. The environment used in this project is in relation to my other project:[Creating a Live SOC/Honeynet in Azure ](https://github.com/James-Jeudy/SOC-Honeynet-Azure).

![NIST 800-61 Framework](https://github.com/James-Jeudy/Incident-Response/assets/160562010/63fb401a-8a87-4293-9c9d-d5cbedb5f2f7)



## Step 1: Preparation:
<h3> This was initiated already by ingesting all of the logs into Log Analytics Workspace and Sentinel and configuring alert rules in my SOC/Honeynet project. 

<h3> We turned off the VM immediately after seeing the alert to prevent any further malicious activity.

![Immediately stop the VM](https://github.com/James-Jeudy/Incident-Response/assets/160562010/a7b25b6e-a034-4e93-ba05-5f2b4dc6fd3e)

## Step 2: Detection & Analysis

## 1. Set Severity, Status, & Owner

<h3> I set the severity to High as it was a successful brute force attempt into my Windows V. I set the status to active as I was currently working on the ticket and the owner as Myself. 

![Set the owner status   severity for incident](https://github.com/James-Jeudy/Incident-Response/assets/160562010/d6a28555-52fc-4fff-a717-8170c46beeac)

## 2. View Full Details

![View full details of the incident](https://github.com/James-Jeudy/Incident-Response/assets/160562010/6b74c7a6-6165-4b94-af83-f459d2ff983d)

## 3. Observe the Activity Log (for history of incident):

![View the activity log](https://github.com/James-Jeudy/Incident-Response/assets/160562010/6510966a-bf70-4e6e-b3df-444a7c4481f1)

![Incident Activity Log](https://github.com/James-Jeudy/Incident-Response/assets/160562010/7b3eae0a-c9d2-4e13-922a-df3b47220eb9)


## 4. Observe Entities and Incident Timelines (are they doing anything else?). Where is the attacker located?

![Incident Timeline](https://github.com/James-Jeudy/Incident-Response/assets/160562010/f143c3b1-6ec2-4bb1-ac8a-836b6b3e27a9)

![Entities for incident person place or thing](https://github.com/James-Jeudy/Incident-Response/assets/160562010/091d9f00-8d08-4084-9380-0a77aac0856c)

![Location of Attacker- further details](https://github.com/James-Jeudy/Incident-Response/assets/160562010/1a24c491-d531-46f3-be28-147ebaeeabb2)

![Attacker IP located in Brooklyn](https://github.com/James-Jeudy/Incident-Response/assets/160562010/31f2b132-ccef-4a1d-bcf3-2c302c52faed)

## 5. “Investigate” the incident and continue trying to determine the scope

![Investigate the incident](https://github.com/James-Jeudy/Incident-Response/assets/160562010/07beacc7-296a-4537-aeeb-6017c27fabdc)

![Investigate incident screen](https://github.com/James-Jeudy/Incident-Response/assets/160562010/557d53b1-05c7-4e2c-a2b8-f9f99aadf284)

## 6. Inspect the entities and see if there are any related events

![Right click attacker IP to get related alerts to see if there is any other malicious activity](https://github.com/James-Jeudy/Incident-Response/assets/160562010/18ec758c-6a56-4362-9471-91c40c40e55d)

![Lots of other brute force success   attempts from this IP address](https://github.com/James-Jeudy/Incident-Response/assets/160562010/330021c1-139f-41b3-8a48-c6d9f17dc4d6)

![Check related alerts of windows VM](https://github.com/James-Jeudy/Incident-Response/assets/160562010/bb246031-2965-4e61-bec4-77e044bafd37)

![This machine is involved with numerous alerts for obvious reasons](https://github.com/James-Jeudy/Incident-Response/assets/160562010/a0401fdb-1198-478d-bdcd-b77d541cdb61)

## 7. Determine legitimacy of the incident (True Positive, False Positive, etc.)


![Take a look at the analytics rule](https://github.com/James-Jeudy/Incident-Response/assets/160562010/3667e850-3b10-4669-9734-12a52913ab79)

![Copy the query](https://github.com/James-Jeudy/Incident-Response/assets/160562010/7b0e94ae-8ff8-47a0-999c-f54c098bedc8)

![Query in log analytics workspace](https://github.com/James-Jeudy/Incident-Response/assets/160562010/a7bdd65a-e4cc-40ad-8a9f-9f3f3343602f)

![As noted there was custom brute force logins with account on machine](https://github.com/James-Jeudy/Incident-Response/assets/160562010/3d976e3e-5580-4c6e-a9b9-6a1498047a0c)

![Brute force success from laptop](https://github.com/James-Jeudy/Incident-Response/assets/160562010/88bae0d4-83ad-4c71-86a5-5a05d69fec4d)

## Step 3: Containment, Eradication, & Recovery:

