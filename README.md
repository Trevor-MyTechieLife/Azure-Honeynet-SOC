# Azure-Honeynet-SOC
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/XKqCUbW.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/eIntffG.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/uBGlTSG.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

## Stage 1
Before the changes, all resources were initially deployed and made accessible to the internet, including Virtual Machines with open Network Security Groups. Additionally, all other resources had public endpoints visible to the internet, making private endpoints unnecessary.

After the changes, the Network Security Groups were strengthened by blocking all traffic except from the admin workstation. Moreover, all other resources were safeguarded by their built-in firewalls and private endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-05-03 02:06:45
Stop Time 2023-05-04 02:06:45

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 107,910
| Syslog                   | 2,514
| SecurityAlert            | 5
| SecurityIncident         | 72
| AzureNetworkAnalytics_CL | 2286

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-05-06 02:06:45
Stop Time	2023-05-07 02:06:45

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 11,010
| Syslog                   | 24
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastially reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
