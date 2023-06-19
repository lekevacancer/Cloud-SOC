# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/QhT9O0P.jpg)
## Introduction

For this project, I built a mini honeynet in Azure to simulate real-world cyber attacks. I ingested log sources from various resources into a Log Analytics workspace, which was then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, applied some security controls to harden the environment, measured metrics for another 24 hours, then showed the results below. The metrics I followed are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Technologies, Regulations, and Azure Components Utilized

- Azure Virtual Network(VNet)
- Azure Network Security Group(NSG)
- Virtual Machines ((2) Windows (Attacker VM, Vulnerable VM), (1) Linux)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Jey Vault for Secure Secrets Managemt
- Azure Storeage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management(SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- WIndows Remote Desktop for Remote Access
- Command Line Interface(CLI) for System Management
- Posershell for Automation and COnfiguration Management
- [NIST SP 800-53 Revision 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final) for Security Control
- [NIST SP 800-61 Revision 2](https://www.nist.gov/privacy-framework/nist-sp-800-61) for Incident Handling Guidance
  
## Methodology

- Creating the Honeynet: Initially, I deployed multiple virtual machines with known vulnerabilities in Azure, creating a simulated insecure environment.
- Monitoring and Analysis: Next, I configured Azure to collect logs from various sources and consolidated them in the Log Analystics Workspace. Using Microsoft Sentinel, I built attack maps, setup alert triggers, and created incidents based on the gathered data.
- Measurement of Secuirty Metrics: During the insecure state of the environment, I monitored and recorded key security metrics for a 24-hour period. This established a baseline for comparision once remediation measures were implemented.
- Incident Response and Remediation: After addressing the identified incidents and vulnerabilities, I proceeded to stregthen the environment by implementing security best practices and following Azure-specific recommendations.
- Analysis After Remediation: Fianlly, in order to evaluate the effectiveness of the implemented measures, I observed the environment for an additional 24-hour period, measuring security metrics once again and compared them against the intial baseline.
  
## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

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
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
