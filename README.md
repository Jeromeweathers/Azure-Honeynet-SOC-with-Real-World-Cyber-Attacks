# Azure-Honeynet-SOC-with-Real-World-Cyber-Attacks

![Cloud Honeynet / SOC](https://github.com/Jeromeweathers/Azure-Honeynet-SOC-with-Real-World-Cyber-Attacks/blob/main/AZUre%20aD.gif)

## Introduction and Objective

Within the Azure platform, a small honeynet was built for this project. The goal was to gather and analyze logs from various sources, which were then compiled in a workspace for log analytics. In order to make use of these data, Microsoft Sentinel was implemented, and it has since been used to generate incidents, attack maps, and alert triggers. Over the course of a day, Azure Sentinel measured the parameters of an unsafe environment. After this stage, security measures were put in place to protect the virtual environment. Finally, a second 24-hour metric measuring phase was carried out, and the outcomes of these efforts are shown below. Analyzed metrics included: 
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)


## Technologies, Azure Components, and Regulations Employed
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
- [NIST SP 800-53 Revision 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) for Security Controls
- [NIST SP 800-61 Revision 2](https://www.nist.gov/privacy-framework/nist-sp-800-61) for Incident Handling Guidance


## Architecture BEFORE Hardening and Implementing Security Controls
![Architecture Diagram](https://i.imgur.com/twZXRq7.png)
During the initial "BEFORE" phase of the project, I strategically set up a virtual environment, which was made accessible to the public with the intention of luring potential malicious actors. The main objective here was to study and analyze their attack patterns. To execute this plan, we deployed a Windows virtual machine hosting a SQL database and a Linux server, ensuring that my network security groups (NSGs) were configured with an "Allow All" policy. Additionally, I went a step further to entice these attackers by deploying a storage account and key vault with public endpoints, making them visible on the open internet.

Throughout this stage, my focus was on monitoring the unsecured environment using Microsoft Sentinel, which expertly utilized logs aggregated by the Log Analytics workspace. This enabled me to gain valuable insights into the activities and techniques employed by these potential threats, thereby reinforcing the security of the system against real-world risks.


## Architecture AFTER Hardening and Implementing Security Controls
![Architecture Diagram](https://i.imgur.com/OT1Ou0f.png)
In the "AFTER" stage, I focused on strengthening security to meet NIST SP 800-53 Rev4 SC-7(3) Access Points. For this, I hardened Network Security Groups (NSGs) by blocking all inbound and outbound traffic, except for designated public IP addresses that needed access to the virtual machines. This ensured only authorized traffic from trusted sources accessed the virtual machines.
- <b>Built-in Firewalls</b>: Azure's built-in firewalls were configured on the virtual machines to restrict unauthorized access and protect the resources from malicious connections. This step involved fine-tuning the firewall rules based on the service and responsibilities of each VM which mitigated the attack surface bad actors had access to.

- <b>Private Endpoints</b>: To bolster Azure Key Vault and Storage Containers' security, I swapped out Public Endpoints with Private Endpoints. This effectively restricted access to these critical resources solely to the virtual network, eliminating any exposure to the public internet.

## Attack Maps Before Hardening / Security Controls
<b>This attack map shows the traffic allowed by a Network Security Group with all traffic allowed inbound</b>
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/RdoU9D0.png)<br>

<b>This attack map shows all the attempts malicious actors attempting to access the Linux virtual machine</b>
![Linux Syslog Auth Failures](https://i.imgur.com/4POzUqi.png)<br>

 <b>This attack map shows all the attempts malicious actors attempting to access the Windows virtual machine</b>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/UKnEJ03.png)<br>


## Metrics Before Hardening / Security Controls
The following table shows the metrics measured within the unsecured environment for 24 hours:
Start Time 2023-06-04 01:56:14
Stop Time 2023-06-05  01:56:14

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 7550
| Syslog                   | 537
| SecurityAlert            | 5
| SecurityIncident         | 263
| AzureNetworkAnalytics_CL | 1155



## Metrics After Hardening / Security Controls

The following table shows the metrics measured in the environment for another 24 hours after applying security controls:
Start Time 6/4/2023, 10:09:54 PM
Stop Time	 6/5/2023, 10:09:54 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 879
| Syslog                   | 84
| SecurityAlert            | 0
| SecurityIncident         | 8
| AzureNetworkAnalytics_CL | 15

## Conclusion

In this project, I created a compact yet effective honeynet in Microsoft Azure, integrating log sources into a Log Analytics workspace. Microsoft Sentinel was set up to trigger alerts and incidents from the ingested logs. I measured metrics in the unsecured environment before and after implementing robust security controls. The results were remarkable – a 88% decrease in Windows Security Events, a 84% drop in Linux Events. I truly believe I can improve even further, and I'm excited to take on the project again to make it even better. It's interesting to note that this project played a significant role in landing me my first SOC Analyst position. I owe a big thank you to Josh Madakor, With this valuable experience, I look forward to honing my skills and contributing even more in the cybersecurity field.
