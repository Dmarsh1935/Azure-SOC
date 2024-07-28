# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet - SOC Diagram](https://github.com/user-attachments/assets/6d037bb4-47c5-4f12-b8e1-100610061815)


## Introduction

In this project, I set up a mini honeynet in Azure to monitor and analyze security events. The goal was to assess the level of activity in an insecure environment over a 24-hour period and compare it with another 24-hour period after implementing security controls from NIST 800-53. To accomplish this, I integrated log data from various sources into an Azure Log Analytics workspace. Microsoft Sentinel was then utilized to analyze the logs, develop attack maps, trigger alerts, and generate incidents. The metrics monitored during the project included:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Technologies Used
- Azure Virtual Network (VNet)
- Azure Network Security Groups (NSG)
- Virtual Machines (2 Windows VMs, 1 Linux VM)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault for Secure Secrets Management
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management
- NIST SP 800-53 Revision 5 for Security Controls
- NIST SP 800-61 Revision 2 for Incident Handling Guidance

## Architecture Before Hardening / Security Controls
![Architecture Before Hardening](https://github.com/user-attachments/assets/977ed9c6-8c9c-4c66-a8ce-19712eca1856)


## Architecture After Hardening / Security Controls
![Architecture After Hardening](https://github.com/user-attachments/assets/ed746a33-e206-4ea4-97e0-7e8a6b8a966e)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls

<b>The NSG Attack Map below is a representation of a Network Security Group that allowed all inbound traffic.</b> <br> </br>

![image](https://github.com/user-attachments/assets/afd4deeb-1e24-421c-89a0-2310e5930e00)
<br> </br>
<br> </br>

<b> The Syslog Attack Map shows all the malicious attempts of accessing the Linux VM over SSH. </b> <br> </br>
![image](https://github.com/user-attachments/assets/42d603fe-6afb-40cb-b8e1-16ceff530996)
<br> </br>
<br> </br>

<b> The final attack map shows all the malicious attempts to RDP into the Windows VM. </b> <br> </br>
![image](https://github.com/user-attachments/assets/3f5b6860-543b-4199-af3d-2334a8b537bf)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours: 
<br> </br>
<b>Start Time 7/25/2024 8:12:59.
<br> </br>
Stop Time 7/26/2024 8:12:59.</b>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 93793
| Syslog                   | 1183
| SecurityAlert            | 36
| SecurityIncident         | 307
| AzureNetworkAnalytics_CL | 2000

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
<br> </br>
<b>Start Time 7/27/2024 8:13:10
<br> </br>
Stop Time	7/28/2024 8:13:10</b>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 21053
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. After implementing some NIST 800-53 security controls, there was a 77% reduction in Windows security events and a 99% reduction in Linux events, and a 100% reduction in security alerts, incidents, and malicious inbound network trafic. 

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
