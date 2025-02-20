# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://imgur.com/FIIRA2X.jpg)

## Azure Topology
![Azure Topology](https://imgur.com/U7G84ng.jpg)

## Introduction

In this project, I designed a honeynet by deploying Virtual Machines and an SQL Database within Microsoft Azure to collect and analyze log data from various resources. The network security groups were left wide open to internet traffic in order to generate logs. These logs were sent to a Log Analytics workspace, where Microsoft Sentinel was utilized to generate attack maps, issue alerts, and track incidents. To evaluate the effectiveness of security measures, I first measured key metrics in the unsecured environment over a 24-hour period. After implementing security hardening measures, I repeated the monitoring for another 24 hours to observe the impact. The metrics analyzed in this project include:

- SecurityEvent: Windows Event Logs
- Syslog: Linux Event Logs
- SecurityAlert: Alerts triggered by Log Analytics
- SecurityIncident: Sentinel-generated incidents
- AzureNetworkAnalytics_CL: Malicious network traffic permitted into the honeynet

The mini honeynet architecture deployed in Azure is comprised of the following key components:

 - Virtual Network (VNet)
 - Network Security Groups (NSGs)
 - Virtual Machines: Two Windows and one Linux instance
 - Log Analytics Workspace
 - Azure Key Vault
 - Azure Storage Account
 - Microsoft Sentinel

## Initial Configuration:

In the initial setup, all resources were deployed with public exposure to the internet. The Virtual Machines had unrestricted access through both their NSGs and built-in firewalls. Additionally, other resources were accessible via public endpoints without utilizing Private Endpoints, leaving the environment fully exposed.<br>

## Hardened Configuration:

Following security enhancements, the NSGs were tightened to block all traffic except from my admin workstation. Virtual Machines were further secured by configuring their built-in firewalls, and all other resources were safeguarded by leveraging Private Endpoints, eliminating public exposure.

## Attack Maps Before Hardening / Security Controls

## NSG Allowed Inbound Malicious Flows
![NSG Allowed Inbound Malicious Flows](https://imgur.com/L4k4IAL.png)<br>

## Linux Syslog Auth Failures
![Linux Syslog Auth Failures](https://imgur.com/iEsfBYn.png)<br>

## Windows RDP/SMB Auth Failures
![Windows RDP/SMB Auth Failures](https://imgur.com/lSepETW.png)<br>

## Microsoft SQL Server Auth Failures
![Microsoft SQL Server Auth Failures](https://imgur.com/nMhlqe1.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

| **Start Time**            | **Stop Time**            |
|----------------------------|--------------------------|
| November 15, 2024, 11:18 PM   | November 16, 2024, 11:18 PM  |

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 60746
| Syslog                   | 10174
| SecurityAlert            | 223
| SecurityIncident         | 223
| AzureNetworkAnalytics_CL | 3738

## Attack Maps Before Hardening / Security Controls

```All map queries returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
| **Start Time**            | **Stop Time**            |
|----------------------------|--------------------------|
| November 18, 2024, 8:49 PM   | November 19, 2024, 8:49 PM  |

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 1559
| Syslog                   | 8
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, I successfully deployed a mini honeynet in Microsoft Azure and integrated its log sources into a centralized Log Analytics workspace. By using Microsoft Sentinel to monitor and analyze these logs, we were able to trigger alerts and create incidents based on suspicious activity. I collected metrics from the unsecured environment both before and after applying hardening measures. The results showed a marked reduction in security events and incidents, clearly demonstrating the effectiveness of the implemented controls.

It’s important to note that if the network had experienced higher levels of legitimate user activity, the number of security events and alerts during the 24-hour period after hardening might have been higher—either due to an increased attack surface or additional logging from normal operations.
